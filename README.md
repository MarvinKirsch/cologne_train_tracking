# cologne_train_tracking

This Django application, `train_tracker`, is designed to track trains using the `v5.db.transport.rest` API, focusing on train schedules, delays, and cancellations. It periodically fetches data from the API and stores the essential information in a PostgreSQL database, minimizing database interactions to reduce costs.

## Setup

1. **Create the App**
   Navigate to your Django project directory and execute:
   ```
   python manage.py startapp train_tracker
   ```

## Models

1. Define your models in `train_tracker/models.py`.
2. Run migrations:
   ```
   python manage.py makemigrations
   python manage.py migrate
   ```

## API Interaction

1. Install `requests` library if not present:
   ```
   pip install requests
   ```
2. Implement API interaction logic in `train_tracker/api.py`.

## Scheduling and Background Tasks

1. Set up Celery following the official documentation.
2. Create a periodic task in `train_tracker/tasks.py` for the API calls.

## Error Handling and Logging

1. Implement error handling in your API interaction functions.
2. Configure Django logging for error tracking.

## Testing

1. Write tests in `train_tracker/tests.py`.

## Documentation

1. Document your code with docstrings and comments for clarity.

## Configuration and Deployment

1. Update your `settings.py` for database and Celery configurations.
2. Ensure your production environment supports Celery workers.

## Deployment

Follow your standard Django deployment process, ensuring support for Celery and the chosen database.

Remember to refer to the official Django, Celery, and `requests` library documentation for detailed instructions and best practices.