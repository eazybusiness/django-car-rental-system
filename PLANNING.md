# Django Car Rental System - Technical Planning

## Architecture Overview

### Technology Stack
- **Backend:** Django 4.2+ with Python 3.10+
- **Database:** SQLite3 (development), PostgreSQL-ready for production
- **Frontend:** Django Templates + Bootstrap 5 + Vanilla JavaScript
- **Admin:** Django Jazzmin for enhanced admin interface
- **Authentication:** Django's built-in auth system

## Project Structure

```
django-car-rental-system/
├── car_rental_project/          # Django project root
│   ├── __init__.py
│   ├── settings.py              # Project settings
│   ├── urls.py                  # Root URL configuration
│   ├── wsgi.py                  # WSGI application
│   └── asgi.py                  # ASGI application
│
├── rentals/                     # Main application
│   ├── migrations/              # Database migrations
│   ├── static/rentals/          # App-specific static files
│   │   ├── css/
│   │   │   └── styles.css
│   │   ├── js/
│   │   │   ├── booking.js
│   │   │   └── availability.js
│   │   └── images/
│   ├── templates/rentals/       # App templates
│   │   ├── base.html
│   │   ├── home.html
│   │   ├── cars/
│   │   │   ├── list.html
│   │   │   └── detail.html
│   │   ├── bookings/
│   │   │   ├── create.html
│   │   │   ├── confirm.html
│   │   │   └── history.html
│   │   └── accounts/
│   │       ├── login.html
│   │       ├── register.html
│   │       └── profile.html
│   ├── __init__.py
│   ├── models.py                # Data models
│   ├── views.py                 # View functions/classes
│   ├── forms.py                 # Form definitions
│   ├── urls.py                  # App URL patterns
│   ├── admin.py                 # Admin configuration
│   ├── signals.py               # Django signals
│   └── utils.py                 # Helper functions
│
├── media/                       # User-uploaded files
│   └── car_images/
│
├── static/                      # Global static files
│   ├── css/
│   ├── js/
│   └── images/
│
├── templates/                   # Global templates
│   └── admin/                   # Admin template overrides
│
├── fixtures/                    # Sample data
│   └── sample_data.json
│
├── tests/                       # Test suite
│   ├── test_models.py
│   ├── test_views.py
│   └── test_forms.py
│
├── .gitignore
├── requirements.txt
├── manage.py
└── README.md
```

## Database Schema

### User Model (Extended)
```python
# Uses Django's built-in User model with Profile extension
Profile:
- user (OneToOne -> User)
- phone_number (CharField)
- address (TextField)
- date_of_birth (DateField)
- driver_license_number (CharField)
- created_at (DateTimeField)
```

### Category Model
```python
Category:
- id (AutoField, PK)
- name (CharField, unique, max_length=50)
- description (TextField)
- slug (SlugField, unique)
- created_at (DateTimeField, auto_now_add)
- updated_at (DateTimeField, auto_now)
```

### Car Model
```python
Car:
- id (AutoField, PK)
- name (CharField, max_length=100)
- model (CharField, max_length=100)
- year (IntegerField)
- category (ForeignKey -> Category, on_delete=CASCADE)
- rental_price_per_day (DecimalField, max_digits=8, decimal_places=2)
- is_available (BooleanField, default=True)
- description (TextField)
- features (JSONField)  # e.g., {"seats": 5, "transmission": "automatic"}
- mileage (IntegerField)
- fuel_type (CharField, choices)
- color (CharField)
- license_plate (CharField, unique)
- image_main (ImageField, upload_to='car_images/')
- image_2 (ImageField, optional)
- image_3 (ImageField, optional)
- image_4 (ImageField, optional)
- created_at (DateTimeField, auto_now_add)
- updated_at (DateTimeField, auto_now)
```

### Booking Model
```python
Booking:
- id (AutoField, PK)
- customer (ForeignKey -> User, on_delete=CASCADE)
- car (ForeignKey -> Car, on_delete=PROTECT)
- start_date (DateField)
- end_date (DateField)
- pickup_location (CharField)
- dropoff_location (CharField)
- total_days (IntegerField, calculated)
- price_per_day (DecimalField)
- total_price (DecimalField, calculated)
- status (CharField, choices: PENDING, CONFIRMED, ACTIVE, COMPLETED, CANCELLED)
- special_requests (TextField, optional)
- created_at (DateTimeField, auto_now_add)
- updated_at (DateTimeField, auto_now)

Constraints:
- end_date > start_date
- Unique together: (car, start_date, end_date) for overlapping prevention
```

## Key Features Implementation

### 1. User Authentication System

**Registration:**
- Custom registration form with profile fields
- Email verification (optional)
- Password strength validation
- Automatic profile creation via signals

**Login/Logout:**
- Django's built-in authentication
- Remember me functionality
- Redirect to intended page after login

**Profile Management:**
- View and edit profile information
- Change password
- View booking history

### 2. Vehicle Inventory Management

**Admin Interface:**
- Django Jazzmin customization
- Bulk actions for availability updates
- Image preview in admin list
- Filter by category, availability, year
- Search by name, model, license plate

**Frontend Display:**
- Grid/list view toggle
- Filter by category, price range, year
- Sort by price, year, name
- Pagination (12 cars per page)
- Quick view modal

### 3. Booking System

**Availability Logic:**
```python
def check_availability(car_id, start_date, end_date):
    """
    Check if a car is available for the given date range.
    
    Returns:
        bool: True if available, False otherwise.
    """
    overlapping_bookings = Booking.objects.filter(
        car_id=car_id,
        status__in=['CONFIRMED', 'ACTIVE'],
        start_date__lt=end_date,
        end_date__gt=start_date
    )
    return not overlapping_bookings.exists()
```

**Booking Flow:**
1. User selects car and dates
2. System checks availability
3. Display price calculation
4. User confirms booking
5. Booking created with PENDING status
6. Admin can confirm/reject
7. Email notification sent

**Price Calculation:**
```python
def calculate_total_price(start_date, end_date, price_per_day):
    """
    Calculate total rental price.
    
    Args:
        start_date (date): Rental start date.
        end_date (date): Rental end date.
        price_per_day (Decimal): Daily rental rate.
    
    Returns:
        tuple: (total_days, total_price)
    """
    total_days = (end_date - start_date).days
    total_price = total_days * price_per_day
    return total_days, total_price
```

### 4. Admin Dashboard

**Django Jazzmin Configuration:**
```python
JAZZMIN_SETTINGS = {
    "site_title": "Car Rental Admin",
    "site_header": "Car Rental Management",
    "site_brand": "Car Rental System",
    "welcome_sign": "Welcome to Car Rental Admin",
    "show_sidebar": True,
    "navigation_expanded": True,
    "icons": {
        "rentals.car": "fas fa-car",
        "rentals.category": "fas fa-tags",
        "rentals.booking": "fas fa-calendar-check",
        "auth.user": "fas fa-users",
    }
}
```

**Custom Admin Actions:**
- Bulk mark as available/unavailable
- Export bookings to CSV
- Send confirmation emails
- Generate reports

### 5. Responsive Design

**Bootstrap 5 Components:**
- Navbar with user menu
- Card-based car listings
- Modal for quick view
- Date picker for bookings
- Form validation
- Alert messages
- Responsive tables

**Mobile Optimization:**
- Mobile-first approach
- Touch-friendly buttons
- Optimized images
- Hamburger menu
- Swipeable car galleries

## URL Structure

```
/                           # Homepage
/cars/                      # Car listing
/cars/<slug>/               # Car detail
/cars/category/<slug>/      # Cars by category
/booking/create/<car_id>/   # Create booking
/booking/confirm/<id>/      # Booking confirmation
/booking/history/           # User's bookings
/accounts/register/         # Registration
/accounts/login/            # Login
/accounts/logout/           # Logout
/accounts/profile/          # User profile
/admin/                     # Admin dashboard
```

## Security Considerations

1. **Authentication & Authorization:**
   - Login required for bookings
   - User can only view/modify own bookings
   - Admin-only access to management features

2. **Data Validation:**
   - Form validation on client and server
   - Date range validation
   - Price calculation server-side only
   - File upload validation (type, size)

3. **CSRF Protection:**
   - Django's built-in CSRF middleware
   - CSRF tokens in all forms

4. **SQL Injection Prevention:**
   - Django ORM parameterized queries
   - No raw SQL without proper escaping

5. **XSS Protection:**
   - Template auto-escaping enabled
   - User input sanitization

## Performance Optimization

1. **Database:**
   - Indexes on frequently queried fields
   - Select_related for foreign keys
   - Prefetch_related for reverse relations
   - Database connection pooling

2. **Static Files:**
   - Collectstatic for production
   - CDN for Bootstrap/jQuery
   - Image optimization
   - Browser caching headers

3. **Queries:**
   - Pagination to limit results
   - Lazy loading for images
   - Query optimization with Django Debug Toolbar

## Testing Strategy

### Unit Tests
- Model methods and properties
- Form validation
- Utility functions
- Signal handlers

### Integration Tests
- Booking flow end-to-end
- User registration and login
- Car availability checking
- Admin actions

### Test Coverage Goals
- Models: 90%+
- Views: 80%+
- Forms: 85%+
- Overall: 85%+

## Deployment Considerations

**Environment Variables:**
```
SECRET_KEY=<django-secret-key>
DEBUG=False
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DATABASE_URL=<production-database-url>
EMAIL_HOST=<smtp-host>
EMAIL_PORT=587
EMAIL_HOST_USER=<email-user>
EMAIL_HOST_PASSWORD=<email-password>
```

**Production Checklist:**
- [ ] DEBUG = False
- [ ] Strong SECRET_KEY
- [ ] ALLOWED_HOSTS configured
- [ ] Database backups enabled
- [ ] Static files served via CDN/nginx
- [ ] Media files properly secured
- [ ] HTTPS enabled
- [ ] Security middleware enabled
- [ ] Error logging configured
- [ ] Performance monitoring

## Dependencies (requirements.txt)

```
Django==4.2.10
Pillow==10.2.0
django-jazzmin==2.6.0
python-decouple==3.8
django-crispy-forms==2.1
crispy-bootstrap5==2.0.0
```

## Development Workflow

1. **Setup:**
   - Create virtual environment
   - Install dependencies
   - Run migrations
   - Create superuser
   - Load fixtures

2. **Development:**
   - Feature branch workflow
   - Write tests first (TDD)
   - Code review before merge
   - Keep migrations clean

3. **Testing:**
   - Run tests before commit
   - Check coverage
   - Manual testing on staging

4. **Deployment:**
   - Merge to main
   - Run migrations
   - Collect static files
   - Restart application server

## Code Style Guidelines

- **Python:** PEP8 compliant
- **Docstrings:** Google style
- **Type Hints:** For function parameters and returns
- **Comments:** Explain why, not what
- **Naming:** Descriptive and consistent
- **Line Length:** Max 100 characters
- **Imports:** Grouped (stdlib, third-party, local)

## Future Enhancements Roadmap

**Phase 2 (Post-MVP):**
- Payment gateway (Stripe/PayPal)
- Email notifications
- SMS reminders
- Advanced search filters
- Customer reviews and ratings

**Phase 3:**
- REST API for mobile app
- Real-time chat support
- Loyalty program
- Seasonal pricing
- Multi-location support

**Phase 4:**
- Mobile app (React Native)
- Vehicle maintenance tracking
- Insurance management
- Analytics dashboard
- Multi-language support
