# Pedrotech REST API Setup

## Installation

1. Install Django and Django REST Framework:
    ```bash
    pip install django djangorestframework
    ```

2. Start a new Django project:
    ```bash
    django-admin startproject <project_name>
    ```

3. Change to the project directory:
    ```bash
    cd <project_name>
    ```

4. Create a new Django app:
    ```bash
    python3 manage.py startapp <app_name>
    ```

## Configuration

1. Open the `settings.py` file in the main project folder and add `rest_framework` and `<app_name>` to the `INSTALLED_APPS` list:
    ```python
    INSTALLED_APPS = [
        # ...
        'rest_framework',
        '<app_name>',
    ]
    ```

2. Create a model (e.g., `User`) in the `<app_name>/models.py` file. 

3. Run migrations to apply the model changes:
    ```bash
    python3 manage.py makemigrations
    python3 manage.py migrate
    ```

## Serialization

1. Create a `serializers.py` file in the `<app_name>` directory:
    ```python
    from rest_framework import serializers
    from .models import User

    class UserSerializer(serializers.ModelSerializer):
        class Meta:
            model = User
            fields = '__all__'
    ```

## Views

1. Modify `views.py` in the `<app_name>` directory:
    ```python
    from rest_framework.decorators import api_view
    from rest_framework.response import Response
    from rest_framework import status

    from .models import User
    from .serializers import UserSerializer

    @api_view(["GET"])
    def get_user(request):
        return Response(UserSerializer({"name": "ahad", "age": 23}))
    ```

## URLs

1. Create a `urls.py` file in the `<app_name>` directory:
    ```python
    from django.urls import path
    from .views import get_user

    urlpatterns = [
        path('users/', get_user, name='get_user'),
    ]
    ```

2. Update the `urls.py` file in the main project directory:
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include('<app_name>.urls')),
    ]
    ```

## Running the Server

1. Start the Django development server:
    ```bash
    python3 manage.py runserver
    ```

Now your REST API should be up and running! You can access it at `http://127.0.0.1:8000/api/users/`.
