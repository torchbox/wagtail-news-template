# Wagtail Starter Kit - Django Project Template

This Django project template is designed for creating Wagtail builds quickly, intended for developers to bootstrap their Wagtail site development using `wagtail start --template=`. The template comes with pre-defined pages, blocks, functionalities, and fixtures to streamline the initial setup process.

## Todo

- [ ] Update fixtures and images to be more generic
- [ ] Add some form of linting
- [ ] Load in fonts locally
- [ ] Add CI tasks
- [ ] Finish or alter the footer's signup feature to link to a form page
- [ ] Add support for wagtail search promotions
- [ ] Style the related pages slideshow component
- [ ] Accessibility,
    - [ ] Resolve contrast issues with button component
    - [ ] Windows High-Contrast mode support
- [ ] Style other block types
- [ ] Style other field types for form page

## Getting Started

1. **Create a Virtual Environment**: Set up a virtual environment to isolate your project dependencies.

    ```bash
    pyenv virtualenv 3.8 myproject
    pyenv activate myproject
    ```

2. **Install Wagtail**: Install the Wagtail CMS package using pip.

    ```bash
    pip install wagtail
    ```

3. **Initialize Project**: Use the Django `startproject` command to create a new project based on the Wagtail Starter Kit template.

    ```bash
    wagtail start myproject --template=<<path or git url>>
    ```

4. **Navigate to Project Directory**: Move into the newly created project directory.

    ```bash
    cd myproject
    ```

5. **Install Project Dependencies**: Install the project's dependencies into a virtual environment.

    ```bash
    python -m venv venv

    source venv/bin/activate  # For Linux / macOS
    venv/bin/Activate.ps1  # For Windows

    pip install -r requirements.txt
    ```

All commands from now on should be run from inside the virtual environment.

6. **Load Dummy Data**: Optionally load in some dummy data, to populate the site with some content.

    ```bash
    make load-data
    ```

7. **Start the Server**: Start the Django development server.

    ```bash
    make start
    ```

8. **Access the Site and Admin**: Once the server is running, you can view the site at `localhost:8000` and access the Wagtail admin interface at `localhost:8000/admin`. Log in with the default credentials provided by :

    - Username: admin
    - Password: password

### Deploying

Once you have your own copy of the template, you can extend and configure it however you like.

To get it deployed, follow the instructions below for your hosting provider of choice.

Don't see your preference here? Contributions are always welcome!

#### fly.io

Before you can deploy to [fly.io](https://fly.io/), you will need an account, as well as the `fly` CLI tool configured locally.

1. In the root directory of your project (the one with a `fly.toml` file), run `fly launch`
   1. When prompted about copying the existing `fly.toml` file to a new app, choose "Yes".
   2. When prompted about continuing the setup in the web UI, or tweak the generated settings, choose "No".
      1. The "Region" will be selected automatically. If you wish to change this, choose "Yes" instead, and modify the region in the browser.
2. Once the launch is successful, you'll need to [generate a secret key](https://realorangeone.github.io/django-secret-key-generator/)
   1. This can be done using `fly secrets set SECRET_KEY=<key>`, or through the web UI.
3. Finally (optional), load in the dummy data, to help get you started
   1. `fly ssh console -u wagtail -C "./manage.py load_initial_data"`

You can now visit your wagtail site at the URL provided by `fly`. We strongly recommend setting strong password for your user.

The database and user-uploaded media are stored in the attached volume. To save costs and improve efficiency, the app will automatically stop when not in use, but will automatically restart when the browser loads.

## Contributing

To customize this template, you can either make changes directly or backport changes from a generated project (via the `wagtail start` command) by following these steps:

1. Create a new project using the provided instructions in the [Getting Started](#getting-started) section.
2. Make changes within the new project.
3. Once you've completed your changes, you'll need to copy them over to the original project template, making sure to:

    3.1. Replace occurrences of `myproject` with `{{ project_name }}`
    
    3.2. Rename the project directory from `myproject` to `project_name` (without double curly brackets this time).
    
    3.3. Wrap template code (`.html` files under the templates directory), with a [verbatim tag](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#std-templatetag-verbatim) or similar [templatetag](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#templatetag) to prevent template tags being rendered on `wagtail start` ([see django's rendering warning](https://docs.djangoproject.com/en/5.0/ref/django-admin/#render-warning)).
5. Update compiled static assets using `npm run build:prod`. 
6. Update fixtures using `make dump-data`

Make sure to test any changes by reviewing them against a newly created project, by following the [Getting Started](#getting-started) instructions again.


Happy coding with Wagtail! If you encounter any issues or have suggestions for improvement, feel free to contribute or open an issue.
