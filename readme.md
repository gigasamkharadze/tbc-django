# Django Project

- ## Custom User
The First step is to create a custom user model. 
We will create a new app called `custom_user` and 
create a custom user model that extends the `AbstractUser` model.

We will also create a custom user manager for our custom user model.

```python
class CustomUser(AbstractUser):
    username = None
    email = models.EmailField(_("email address"), unique=True)

    USERNAME_FIELD = "email"
    REQUIRED_FIELDS = []

    objects = CustomUserManager()

    def __str__(self):
        return self.email
```

```python
class CustomUserManager(BaseUserManager):
    """
    Custom user model manager where email is the unique identifiers
    for authentication instead of usernames.
    """
    def create_user(self, email, password, **extra_fields):
        """
        Create and save a user with the given email and password.
        """
        if not email:
            raise ValueError(_("The Email must be set"))
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save()
        return user

    def create_superuser(self, email, password, **extra_fields):
        """
        Create and save a SuperUser with the given email and password.
        """
        extra_fields.setdefault("is_staff", True)
        extra_fields.setdefault("is_superuser", True)
        extra_fields.setdefault("is_active", True)

        if extra_fields.get("is_staff") is not True:
            raise ValueError(_("Superuser must have is_staff=True."))
        if extra_fields.get("is_superuser") is not True:
            raise ValueError(_("Superuser must have is_superuser=True."))
        return self.create_user(email, password, **extra_fields)
```

Here we observe the following:
1. We have created a custom user model that extends the `AbstractUser` model.
2. username field is set to None and email field is set to `models.EmailField(_("email address"), unique=True)`.
3. `USERNAME_FIELD` is set to `email` and `REQUIRED_FIELDS` is set to an empty list.
4. We have created a custom user manager that extends the `BaseUserManager` class.
5. `create_user` method is overridden to create a user with the given email and password.
6. `create_superuser` method is overridden to create a superuser with the given email and password.

Then, we implement a form for the custom user model to be used in the admin panel.

![img.png](img.png)


## Installation
Clone the repository to your local machine using the following command:
```bash
git clone https://github.com/gigasamkharadze/tbc-django.git
```
Install the required packages using the following command:
```bash
pip install -r requirements.txt
```
Run the server using the following command:
```bash
python manage.py runserver
```