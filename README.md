# Flask Registration Form

## What this project is

A simple user registration form built with Flask. Users can fill in their name,
email and password. The data gets saved to a SQLite database. There is also a
page that shows all registered users.

---

## How to run it

1. Install the required packages:
```
pip install flask flask-sqlalchemy flask-wtf wtforms email-validator
```

2. Run the app:
```
python app.py
```

3. Open your browser and go to:
```
http://127.0.0.1:5000
```

---

## Pages

- `/register` - the registration form
- `/success/<name>` - shown after successful registration
- `/users` - shows all users stored in the database

---

## Files

```
flask_registration/
├── app.py                   - main Flask app, routes, model, form
├── static/
│   └── css/
│       └── style.css        - basic styling
├── templates/
│   ├── register.html        - registration form page
│   ├── success.html         - shown after registering
│   └── users.html           - table of all users
└── README.md
```

---

## What the form validates

- Name must be at least 2 characters
- Email must be a valid email format
- Password must be at least 6 characters
- Confirm password must match password
- All fields are required
- Email must not already exist in the database

---

## Lessons Learned

The hardest part for me was understanding how Flask-WTF and SQLAlchemy work together.
At first I kept getting a 400 error when submitting the form and I had no idea why.
After a lot of googling I found out it was because I forgot the `{{ form.hidden_tag() }}`
line in the template which is needed for CSRF protection.

I also had a bug where the password was not being saved to the database. I had the
password field in the form but I forgot to include it when creating the User object.
The fix was simple - just add `password=form.password.data` when creating the new_user.

Another thing that confused me was the difference between GET and POST requests.
I kept wondering why my form data was empty after submission until I understood I
needed to check `form.validate_on_submit()` and that the route needs
`methods=['GET', 'POST']`.

Setting up the database also took me a while. I did not know I had to call
`db.create_all()` first otherwise it would throw errors about the table not existing.
