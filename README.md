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
3. models.py: 
    ```
    from django.db import models

    class Train(models.Model):
        train_number = models.CharField(max_length=50)
        train_type = models.CharField(max_length=50)
        # Add more fields as provided by the API

    class Station(models.Model):
        name = models.CharField(max_length=100)
        station_code = models.CharField(max_length=20)
        # Additional station details

    class Journey(models.Model):
        train = models.ForeignKey(Train, on_delete=models.CASCADE)
        origin_station = models.ForeignKey(Station, related_name='departure_station', on_delete=models.CASCADE)
        destination_station = models.ForeignKey(Station, related_name='arrival_station', on_delete=models.CASCADE)
        scheduled_departure = models.DateTimeField()
        actual_departure = models.DateTimeField()
        scheduled_arrival = models.DateTimeField()
        actual_arrival = models.DateTimeField()
        status = models.CharField(max_length=100)  # e.g., On Time, Delayed, Cancelled
        delay_duration = models.IntegerField(null=True, blank=True)  # Delay in minutes
    ```

## API Interaction

1. Install `requests` library if not present:
   ```
   pip install requests
   ```
2. Implement API interaction logic in `train_tracker/api.py`.

3. api.py:
    ```
    import requests
    from .models import Train, Station, Journey

    def fetch_train_data():
        # Placeholder for the API endpoint URL
        api_url = 'https://v5.db.transport.rest/api_endpoint'

        response = requests.get(api_url)
        if response.status_code == 200:
            data = response.json()
            # Parse and process the data
            # Example: create_or_update_train(data)
        else:
            # Handle errors

    # Define more functions as needed to interact with different endpoints
    ```

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

2. Project structure:
    ```
    Django Train Tracker Project
    │
    ├── train_tracker
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── api.py             # Custom module for API interaction
    │   ├── apps.py
    │   ├── models.py          # Models for Train, Station, Journey, etc.
    │   ├── tasks.py           # Celery tasks for periodic updates
    │   ├── tests.py           # Tests for your application
    │   ├── urls.py
    │   └── views.py           # Views for handling business logic
    │
    ├── project_name          # Your main project directory
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── settings.py        # Django settings, include Celery config
    │   ├── urls.py
    │   └── wsgi.py
    │
    ├── manage.py
    ├── requirements.txt      # File containing all the dependencies
    └── README.md             # Project documentation
    ```

3. tasks.py:
    ```
    from celery import shared_task
    from .api import fetch_train_data

    @shared_task
    def fetch_train_data_task():
        fetch_train_data()
        # Process and temporarily store the data

    @shared_task
    def save_data_to_db_task():
        # Access the temporarily stored data
        # Save or update the data in the database
    ```

## Configuration and Deployment

1. Update your `settings.py` for database and Celery configurations.
2. Ensure your production environment supports Celery workers.

## Deployment

Follow your standard Django deployment process, ensuring support for Celery and the chosen database.

Remember to refer to the official Django, Celery, and `requests` library documentation for detailed instructions and best practices.