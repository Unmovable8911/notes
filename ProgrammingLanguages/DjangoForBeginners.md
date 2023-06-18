# 1-4 Very Basics
## Import Hints:
| object | from... |
| ------ | ------- |
| path   | django.urls |
| include | django.urls |
| httpRequest | django.http |
| models | django.db |
| admin | django.contrib |
| ListView | django.views.generic |
| TemplateView | django.views.generic |

## Code Snippets:
Initial Setup
```bash
$ python -m venv .venv
$ source .venv/bin/activate
(.venv)$ pip install django
(.venv)$ django-admin startproject project .
(.venv)$ deactivate
```

```powershell
> python -m venv .venv
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> .\.venv\Script\Activat.ps1
```

```bash
$ git config --global user.name "Your User Name"
$ git config --global user.email "yourname@example.com"
$ git config --global init.defaultBranch main
```

+++++ *Split* +++++
```python
# urls.py
path("/", HomePageView.as_view(), name="home")
path("/", include("<app_name>.urls"))

# models.py
class Post(models.Model):
    text = models.TextField()

# <app>/admin.py
admin.site.register(<model>) # register models to admin site

# views.py
class HomePageView(ListView):
    model = Post
    template_name = "home.html"
```

```bash
python manage.py createsuperuser
python manage.py makemigrations <app_name>
python manage.py migrate
```

## Settings:
```python
INSTALLED_APPS = [
    "...",
    "<app_name>.apps.\u<app_name>Config",
    "...",
]
TEMPLATES = [
    {
        "DIRS": [BASE_DIR / "templates"]
    }
]
```

## Template Tags:
```
# general form of template tags
{{ var }}
{% <keyword> [arg] %}

"{% url 'home' %}"
{% block <block_name> %}{% endblock <block_name> %}
{% extends "<template_name>" %}
{% for <var> in <iterable> %} {% endfor %}
```

## Bullet Points:
- Files Created When a Django Project Starts
    - `__init__.py`: indicates that the this folder is a Python package, Python
      files in this folder can be imported.
    - `asgi.py`: Allows for optional Asynchronous Server Gateway Interface to be
      run.
    - `settings.py`: Controls the overall settings of the project.
    - `urls.py`: Determines which pages to build in respond to URL request.
    - `wsgi.py`: Web Server Gateway Interface, serve eventual web pages.
- The Django Model-View-Template pattern
    - **Model**: Manages data and core business logic.
    - **View**: Describes which data in sent to the user but not its
      presentation.
    - **Template**: Presents the data as HTML with optional CSS, JavaScript, and
      Static Assets.
    - **URL Configuration**: Regular-Expression components configured to a View.
- FIles Created when starts an app
    - `admin.py`: Configuration file for the Django built in Admin app.
    - `models.py`: Defines database models.
    - `views.py`: Handle the request/response logic.
    - `apps.py`: Configuration file for the app.
    - `migrations/`: Keeps track of changes of models.
    - `tests.py`: For app specific tests.
- Use GCBVs when possible, and use CBVs and FBVs when required.
- Default directory where Django looks for template files:
`$PROJECT/<app_name>/templates/<app_name>`.
- Remenber to synchronize database with changes to models.
- Alway include the app name when executing `makemigrations`.
- `ListView` returns a context variable named as `<model>_list`
- `context = {"<identifier_in_template>", object}`  
    `return render(request, "<template_name>", context)`
--------------------------------------------------------------------------------

# 5-7 Blog App
## Import Hints:
| object | from... |
| ------ | ------- |
| DetailView | django.views.generic |
| CreateView | django.views.generic.edit |
| UpdateView | django.views.generic.edit |
| DeleteView | django.views.generic.edit |
| UserCreationForm | django.contrib.auth.forms |

## Code Snippets:
```python
# urls.py
path("post/<int: pk>", PostDetailView.as_view(), name="detail")
path("accounts/", include("django.contrib.auth.urls"))

# views.py
class BlogCreateView(CreateView):
    model = Post
    template_name = "post_create.html"
    fields = ["title", "auth", "body"] # forms involved in template

class SignUpView(CreateView):
    form_class = UserCreationForm
    success_url = reverse_lazy("login")
    template_name = "registeration/signup.html"
```

```html
<!-- templetes/post_new.html !-->
<form method="post">{% csrf_token %}
  {{ form.as_p }}
  <input type="submit" value="Save">
</form>

<!-- templates/delete_post.html !-->
<form method="post">{% csrf_token %}
  <p>Delete? For real?</p>
  <input type="submit" value="Yes! For sure!">
</form>

<a href="{% url 'login' %}">Login</a>
<a href="{% url 'logout' %}">Login</a>
```

## Settings:
```python
LOGIN_REDIRECT_URL = "home"
LOGOUT_REDIRECT_URL = "home"

STATIC_URL = "static/"
STATICFILES_DIRS = [BASE_DIR / "static"]

STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFIELS_STARAGE = "django.contrib.staticfiles.storage.StaticFilesStorage"
# ↑ also "whitenoise.storage.CompressedManifestStaticFilesStorage"

INSTALLED_APPS = [
    "...",
    "whitenoise.runserver_nostatic",
    # ↓ right before the built-in static files engine
    "django.contrib.staticfiles",
    "...",
]
MIDDELWARE = [
    "...",
    "whitenoise.middelware.WhiteNoiseMiddelware",
    "...",
]
```

## Template Tags:
```
{% load static %}
"{% static 'css/base.css' %}"

{% if user.is_authenticated %}
{% else %}
{% endif %}
```

## Bullet Points:
- By default, Django will look for static files (css, js, image, etc.) in
  `$PROJECT/<app_name>/static/`
- Django will look within `templates/registration/login.html` for login form,
  also with logout form.
- The path of the app which manipulates user creation should be listed right
  after the buit-in `auth` app in urls.py.
- The reason to use `reverse_lazy` is that for all GCBVs, the URLs are not
  loaded when the file is imported, so we have to use the lazy form of `reverse`
  to load them later when they are available.
- Django won't serve static files in production.
    - Set `STATIC_ROOT`, which is the absolute location of the collected static
      files
    - Set `STATICFILES_STORAGE`, which is the file engine used to compile static
      files.
    - Execute `python manage.py collectstatic` to compile static files
- `collectstatic` must be run before deployment.
- There are many static files engines to serve static files, the most common
  used one is `WhiteNoise`.
    - Add `whitenoise` to `INSTALLED_APPS` **above** the built-in `staticfiles`.
    - Add `WhiteNoiseMiddelware` to `MIDDLEWARE`.
    - Change `STATICFILES_STORAGE` to use `WhiteNoise`.

# 8-15 Newspaper App
## Import Hints:
| object | from... |
| ------ | ------- |
| AbstractUser | django.contrib.auth.models |
| UserCreationForm | django.contrib.auth.forms |
| UserChangeForm | django.contrib.auth.forms |
| UserAdmin | django.contrib.auth.admin |
| settings | django.conf |
| LoginRequiredMixin | django.contrib.auth.mixins |
| UserPassesTestMixin | django.contrib.auth.mixins | 

## Code Snippets:
```python
# accounts/models.py
class CustomUser(AbstractUser):
    age = models.PositiveIntegerField(null=True, blank=True)

# accounts/forms.py
class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm):
        model = CustomUser
        # fields = UserCreationForm.Meta.fields + ("age",)
        fields = [
            "username",
            "email",
            "age",
        ] # controls which fields are displayed in the admin site

# accounts/admin.py
class CostomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_diplay = [
        "email",
        "username",
        "age",
        "is_staff",
    ] # Control which fields are displayed on the signup page
    # Add "age" field to the user editing form ↓
    fieldsets = UserAdmin.fildsets + ((None, {"fields": ("age",)}),)
    # Add "age" field to the user creation form ↓
    add_fieldsets = UserAdmin.add_fildsets + ((None, {"fields": ("age")},)

admin.site.register(CustomUser, CustomUserAdmin)

# articles/models.py
class Articel(models.Model):
    # ...
    author = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
    )
    
    def get_absolute_url(self):
        return reverse("article_datail", kwargs={"pk":self.pk})

# articles/views.py
# class ArticelCreate(CreateView):
class ArticelCreate(LoginRequiredMixin, CreateView):
    model = Articel
    template_name = "article_create.html"
    fields = ["title", "body"]

    def form_valid(self, form):
        """
        This method enables automatic filling author field with the current
        requesting user
        """
        form.instance.author = self.request.user
        return super().form_valid(form)

# The inherit order matters
class ArticleUpdate(LoginRequiredMixin, UserPassesTestMixin, UpdateView):
    # ...
    def test_func(self):
        """
        checks whether the author of the current object matches the user who is
        requesting the page
        """
        obj = self.get_object()
        return object.author == self.request.user

# articles/admin.py
class CommentInline(admin.TabularInline): # also StackInline
    model = Comment

class ArticleAdmin(admin.ModelAdmin):
    inlines = [CommentInline]

admin.site.register(Articel, ArticelAdmin)
```

## Settings:
```python
AUTH_USER_MODEL = "accounts.CustomUser"

# this one outputs the email message in the console
EMAIL_BACKEND = "django.core.mail.backends.console.EmailBackend"

# this one is for production, service provided by SendGrid
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackend"
DEFAULT_FROM_EMAIL = "your_custom_email_account_to_send_email_to_user"
EMAIL_HOST = "smtp.sendgrid.net"
EMAIL_HOST_USER = "apikey"
# For security reasons, password shoud not be saved in plain text in production ↓
EMIAL_HOST_PASSWORD = "sendgrid_password"
EMAIL_PORT = 587
EMAIL_USE_TLS = True

# Remember to update the timezone
TIME_ZONE = "America/New_York"
```

## Template Tags:
```html
{% if user.is_authenticated %}
```

## Bullet Points:
- Always use a custom user model
- It's important to `migrate` the models after the user model is created.
- If a field has `null=True` it can store a database entry as `NULL`, meaning no
  value.
- If a field has `blank=True` then the form will allow an empty value.
- Two ways to add Bootstrap to project, *locally* or through *Content Delivery
  Network* (CDN).
- The change password page is by default located at
  `.../accounts/password_change`.
    - `templates/registration/password_change_form.html`: customize the change
      password page.
    - `templates/registration/password_change_done.html`: customize the password
      change success page.
- The default URL for reset password is `.../accounts/password_reset/`.
    - `templates/registration/password_reset_form.html`: the page asks for email
    - `templates/registration/password_reset_done.html`: email is sent
    - `templates/registration/password_reset_confirm.html`: new password
    - `templates/registration/password_reset_complete.html`: end of resetting
- The email used for email service should not be the same with the email of the
  superuser account.
- A user defined APP name should not be the same with the names of Django
  built-in apps, which you can check out in `setting.py` `INSTALL_APPS` section.
- There are two ways to refer to a custom user model:
    - `AUTH_USER_MODEL`: used within `models.py`
    - `get_user_model`: anywhere else
- the `get_absolute_url` method of a model is used to tell Django how to calculate
  the canonical URL for an object. This method should appear to return a string
  that can be used to refer to the object over HTTP
- If you delete a model object, the primary key of the objects after it doesn't
  automatically decrease to fill the gap.
- To access the model objects connected to the object using ForeignKeys, use the
  notation as `\L<model>_set`. For example, `article.comment_set.all` gets all
  the comments attached to article
