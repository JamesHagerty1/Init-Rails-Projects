These instructions help you initialize new Rails projects and start developing
them locally. They are useful when it is not convenient to install Rails on your
machine.

Build an image from `Dockerfile.init`:
```bash
docker build -t init-rails-projects -f Dockerfile.init .
```

The image created from `Dockerfile.init` is only used to create new Rails
projects.

Change your present working directory to the one you would like your Rails 
project to live under. Initialize a new Rails project in your local machine's 
present working directory:
```bash
docker run -it --rm -v "$(pwd):/workspace" init-rails-projects rails new your-rails-project-name
```

Copy the `Dockerfile.dev` file from this repo into your new Rails project 
folder, at the root of the project.

Copy the remaining instructions to your Rails project's `README.md`. Follow them
when you want to create a local development environment for your project and
start running the project locally. The Local Development instructions are to be
executed within the Rails project's root directory, not here. Replace 
`your-dev-image-name` in the commands below with a name of your choosing.

### Local Development

Build the image providing a container environment for your project:
```bash
docker build -t your-dev-image-name -f Dockerfile.dev .
```

Enter into the dev container's shell, and expose port 3000 to the host machine, 
because `bin/rails server` binds to port 3000:
```bash
docker run -it --rm -p 3000:3000 -v "$(pwd):/rails_project" your-dev-image-name bash
```

Once within the dev container's shell, run the Rails server (bound to all
network interfaces so that it can be accessed from outside of the container):
```bash
bin/rails server -b 0.0.0.0
```
