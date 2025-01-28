# Wagtail Starter Kit - Django Project Template

This Django project template is designed for creating Wagtail builds quickly, intended for developers to bootstrap their Wagtail site development using `wagtail start --template=`. The template comes with pre-defined pages, blocks, functionalities, and fixtures to streamline the initial setup process.

## Getting Started

1. **Check that you have an appropriate version of Python 3** You want to make sure that you have a [compatible version](https://docs.wagtail.org/en/stable/releases/upgrading.html#compatible-django-python-versions) installed:

   ```sh
   python --version
   # Or:
   python3 --version
   # **On Windows** (cmd.exe, with the Python Launcher for Windows):
   py --version
   ```

2. **Create a Virtual Environment**: Set up a virtual environment to isolate your project dependencies. These instructions are for GNU/Linux or MacOS, but there are [other operating systems in the Wagtail docs](https://docs.wagtail.org/en/stable/getting_started/tutorial.html#create-and-activate-a-virtual-environment).

   ```bash
   python -m venv myproject/env
   source myproject/env/bin/activate
   ```

3. **Navigate to Project Directory**: Move into the newly created project directory.

   ```bash
   cd myproject
   ```

4. **Install Wagtail**: Install the Wagtail CMS package using pip.

   ```bash
   pip install wagtail
   ```

5. **Initialize Project**: Use the `wagtail start` command to create a new project based on the Wagtail Starter Kit template.

   ```bash
   wagtail start --template=https://github.com/torchbox/wagtail-news-template/archive/refs/heads/main.zip myproject .
   ```

6. **Install Project Dependencies**: Install the project's dependencies into a virtual environment.

   ```bash
   pip install -r requirements.txt
   ```

All commands from now on should be run from inside the virtual environment.

8. **Load Dummy Data**: Load in some dummy data to populate the site with some content.

   ```bash
   make load-data
   ```

9. **Start the Server**: Start the Django development server.

   ```bash
   make start
   ```

10. **Access the Site and Admin**: Once the server is running, you can view the site at `localhost:8000` and access the Wagtail admin interface at `localhost:8000/admin`. Log in with the default credentials provided by :

    - Username: admin
    - Password: password

### Deploying

Once you have your own copy of the template, you can extend and configure it however you like.

To get it deployed, follow the instructions below for your hosting provider of choice.

Don't see your preference here? Contributions are always welcome!

#### fly.io

Before you can deploy to [fly.io](https://fly.io/), you will need an account and the `fly` CLI tool will need to be [installed on your machine](https://fly.io/docs/flyctl/install/).

1. In the root directory of your project (the one with a `fly.toml` file), run `fly launch`
   1. When prompted about copying the existing `fly.toml` file to a new app, choose "Yes".

> [!CAUTION]
> Choosing "No" (the default) here will result in a broken deployment, as the `fly.toml` file requires configuration needed for the project to run correctly.

2. When prompted about continuing the setup in the web UI, or tweak the generated settings, choose "No".
   1. The "Region" will be selected automatically. If you wish to change this, choose "Yes" instead, and modify the region in the browser.
3. Once the launch is successful, you'll need to [generate a secret key](https://realorangeone.github.io/django-secret-key-generator/)
   1. This can be done using `fly secrets set SECRET_KEY=<key>`, or through the web UI.
4. Finally (optional), load in the dummy data, to help get you started
   1. `fly ssh console -u wagtail -C "./manage.py load_initial_data"`

> [!NOTE]
> If you receive "error connecting to SSH server" when running the above command, It likely means the `fly.toml` above wasn't picked up correctly. Unfortunately, you'll need to delete your application and start again, resetting the changes to the `fly.toml` file.
> If the error still persists, check the application logs.

You can now visit your wagtail site at the URL provided by `fly`. We strongly recommend setting strong password for your user.

The database and user-uploaded media are stored in the attached volume. To save costs and improve efficiency, the app will automatically stop when not in use, but will automatically restart when the browser loads.

#### Divio Cloud

[![Deploy to Divio](https://docs.divio.com/deploy-to-divio.svg)](https://control.divio.com/app/new/?template_url=https://github.com/divio/getting-started-with-wagtail/archive/refs/heads/main.zip)

Easily deploy your application to [Divio Cloud](https://www.divio.com/) using the steps below:

1. **Getting Started**  
   Follow the [Getting Started](#getting-started) instructions to set up your project locally.

2. **Prepare Your Repository**  
   Upload your project to GitHub or another Git provider.

3. **Create a New Application**  
   Log in to the [Divio Control Panel](https://control.divio.com/) and create a new application and

   - Choose "**I already have a repository**.".
   - Connect your Git provider and proceed by clicking "**Next**.".
   - Give your application a suitable name and select the "**Free Trial**" plan to get started.

4. **Configure a Database**
   You need a [database](https://docs.divio.com/introduction/aldryn-django/django-05-database/) in order for the deployment to succeed. Adapt your [`settings/base.py`](https://github.com/torchbox/wagtail-news-template/blob/main/project_name/settings/base.py#L105-L111) to include the [proper configuration](https://github.com/divio/getting-started-with-wagtail/blob/main/mysite/settings/base.py#L90-L98). Commit the changes and push them to your repository.

5. **Deploy Your Application**  
   Divio automatically provide **Test** and **Live** environments. From the "Environments" view, click "**Deploy**" on the **Test** environment. Once the deployment completes, access your site using the "Env URL" link.

6. **Additional Configuration**  
   **Migrations and Environment Variables**: Add a "Release command" within the **Settings** section with the value `python manage.py migrate` to run migrations automatically on every deployment. You can add additional commands as needed. The **Env Variables** section allows you to set variables such as `SECRET_KEY` for the test and live environments.

   **Media Storage**: Divio provides [object storage](https://docs.divio.com/reference/work-media-storage/) to store user-uploaded files. You can configure this within your [`settings.py`](https://github.com/divio/getting-started-with-wagtail/blob/main/mysite/settings/base.py#L151-L169).

## Contributing

To customize this template, you can either make changes directly or backport changes from a generated project (via the `wagtail start` command) by following these steps:

1. Create a new project using the provided instructions in the [Getting Started](#getting-started) section.
2. Make changes within the new project.
3. Once you've completed your changes, you'll need to copy them over to the original project template, making sure to:

   3.1. Replace occurrences of `myproject` with `{{ project_name }}`

   3.2. Rename the project directory from `myproject` to `project_name` (without double curly brackets this time).

   3.3. Wrap template code (`.html` files under the templates directory), with a [verbatim tag](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#std-templatetag-verbatim) or similar [templatetag](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#templatetag) to prevent template tags being rendered on `wagtail start` ([see django's rendering warning](https://docs.djangoproject.com/en/5.0/ref/django-admin/#render-warning)).

4. Update compiled static assets using `npm run build:prod`.
5. Update fixtures using `make dump-data`

Make sure to test any changes by reviewing them against a newly created project, by following the [Getting Started](#getting-started) instructions again.

Happy coding with Wagtail! If you encounter any issues or have suggestions for improvement, feel free to contribute or open an issue.
