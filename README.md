# Dockerize Rails Application With PostgreSql

1 In the root directory of your Rails app, create a file named `Dockerfile` (without any file extensions) with the following content:

```
# Use the official Ruby image as the base image
FROM ruby:3.2.2

# Set the working directory inside the container
WORKDIR /app

# Copy the Gemfile and Gemfile.lock into the container
COPY Gemfile Gemfile.lock ./

# Install Ruby dependencies
RUN gem install bundler && bundle install

# Copy the rest of the application code into the container
COPY . .

# Start the Rails server
CMD ["rails", "server", "-b", "0.0.0.0"]

```

2 Create a `docker-compose.yml` file in the root directory of your Rails app with the following content:

```
version: '3'
services:
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: your_postgres_username
      POSTGRES_PASSWORD: your_postgres_password
      POSTGRES_DB: your_postgres_database_name
    volumes:
      - postgres-data:/var/lib/postgresql/data

  web:
    build: .
    command: bundle exec rails s -b 0.0.0.0
    ports:
      - "3000:3000"
    depends_on:
      - db
    volumes:
      - .:/app
    environment:
      DATABASE_URL: postgres://your_postgres_username:your_postgres_password@db/your_postgres_database_name

volumes:
  postgres-data:

```

3 Make sure to replace your_postgres_username, your_postgres_password, and your_postgres_database_name with your actual PostgreSQL credentials.

4 Build the Docker containers
`docker-compose build`

5 Start the Docker containers
`docker-compose up`

6 Setup and Migrate the Database:
In a new terminal tab/window, run the following command to create and migrate the database:

`docker-compose run web bundle exec rake db:setup`

`docker-compose run web bundle exec rake db:migrate`

7 Now your Rails app should be accessible at http://localhost:3000. You can interact with it just like a regular Rails app running on your local machine.
That's it! You've successfully dockerized your Rails app with a local PostgreSQL database. You can stop the Docker containers by pressing Ctrl+C in the terminal running docker-compose up.

Remember that this is a basic setup, and for production use, you may need to consider additional configurations like environment variables, data persistence, and security.

8 Other Cmd For Docker
  
  Remove Docker Volume `docker system prune --volumes`

  Run Rails CMD `docker-compose run web bundle exec rails g scaffold artist name`
