# **Deep Understanding of Models and Migrations in Django**

## **What Happens Behind the Scenes?**
Django’s **Models** define the structure of your database, and **Migrations** apply those changes to the actual database.

Here's how the full process works:

1. **Define Models** → You create a Python class to represent a database table.
2. **Generate Migrations** → Django generates a script that describes changes to apply.
3. **Apply Migrations** → Django executes those scripts to modify the database.

---

## **Step-by-Step Process of Models and Migrations in Django**

### **1. Creating a Django Project and App**
Before working with models, you need a Django project and app.

#### **Create a Django Project**
```sh
django-admin startproject myproject
cd myproject
```

#### **Create an App**
```sh
python manage.py startapp myapp
```
🔹 This creates a folder `myapp/` where all your app-specific files live.

---

### **2. Defining a Model (Creating a Database Table)**
Now, define a model inside `myapp/models.py`:

```python
from django.db import models

class Person(models.Model):  
    name = models.CharField(max_length=20)
    email = models.EmailField()
    phone = models.CharField(max_length=20)
```

#### **What Happens Here?**
- `Person` class represents a **table** in the database.
- Each attribute represents a **column**:
  - `name`: A `VARCHAR(20)` field (text).
  - `email`: A `VARCHAR(254)` field, ensuring proper email format.
  - `phone`: A `VARCHAR(20)` field for phone numbers.

---

### **3. Register the App in Django**
Before running migrations, Django needs to know about `myapp`. Add it to `INSTALLED_APPS` in `settings.py`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',  # Add your app here
]
```

---

### **4. Generating Migrations**
Now that the model is ready, generate a migration file.

Run:
```sh
python manage.py makemigrations
```

🔹 Django creates a migration script inside `myapp/migrations/`, like:
```bash
myapp/migrations/0001_initial.py
```

Inside this file, you’ll see:

```python
from django.db import migrations, models

class Migration(migrations.Migration):

    initial = True

    dependencies = []

    operations = [
        migrations.CreateModel(
            name='Person',
            fields=[
                ('id', models.AutoField(primary_key=True)),
                ('name', models.CharField(max_length=20)),
                ('email', models.EmailField()),
                ('phone', models.CharField(max_length=20)),
            ],
        ),
    ]
```
✅ **This script tells Django to create a `person` table in the database.**

---

### **5. Applying Migrations (Creating the Table)**
To apply the changes to the database, run:
```sh
python manage.py migrate
```

#### **What Happens Now?**
- Django reads the migration script.
- It executes SQL commands to create the `person` table inside the database.

---

### **6. Checking Database Changes**
To check what SQL Django generated for a migration, run:
```sh
python manage.py sqlmigrate myapp 0001_initial
```

Output:
```sql
BEGIN;
CREATE TABLE "myapp_person" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
    "name" varchar(20) NOT NULL, 
    "email" varchar(254) NOT NULL, 
    "phone" varchar(20) NOT NULL
);
COMMIT;
```
✅ **Django converts Python models into SQL tables automatically!**

---

### **7. Modifying a Model**
Let’s say we want to:
1. Rename `name` to `person_name`
2. Add a new field `age`

#### **Modify `models.py`**:
```python
class Person(models.Model):  
    person_name = models.CharField(max_length=20)  # Renamed field
    email = models.EmailField()
    phone = models.CharField(max_length=20)
    age = models.IntegerField(null=True, blank=True)  # New field
```

#### **Generate a New Migration**
```sh
python manage.py makemigrations
```

Migration file created:
```bash
myapp/migrations/0002_rename_name_person_person_name.py
myapp/migrations/0003_person_age.py
```

#### **Apply Changes**
```sh
python manage.py migrate
```
✅ **Now, the table structure in the database is updated!**

---

### **8. Checking Migration Status**
To see which migrations have been applied:
```sh
python manage.py showmigrations
```

Output:
```css
myapp
[X] 0001_initial
[X] 0002_rename_name_person_person_name
[X] 0003_person_age
```
✅ **Migrations with `[ ]` haven’t been applied yet!, and the migrations with `[X]` aplied (migrated)**

---

### **9. Rolling Back Migrations**
If you made a mistake and want to **undo** the last migration:
```sh
python manage.py migrate myapp 0002_rename_name_person_person_name
```
✅ **Now, Django removes the `age` column and reverts back to the previous state.**

---

### **10. Django ORM: Using Models**
#### **Creating a Record**
```python
from myapp.models import Person

person = Person(person_name="John Doe", email="john@example.com", phone="123456789")
person.save()  # Saves to the database
```

#### **Retrieving Records**
```python
people = Person.objects.all()  # Get all records
john = Person.objects.get(person_name="John Doe")  # Get specific record
```

#### **Updating a Record**
```python
john.age = 30
john.save()
```

#### **Deleting a Record**
```python
john.delete()
```
✅ **Django provides a high-level way to work with the database using Python!**

---

### **Final Thoughts**
🔹 **Django models define tables.**  
🔹 **Migrations track changes and apply them to the database.**  
🔹 **Django ORM allows easy database interactions.**  
🔹 **Migrations provide version control and can be rolled back.**  

🚀 **Now you have a deep understanding of models and migrations in Django!**
