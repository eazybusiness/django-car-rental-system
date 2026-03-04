# Django Car Rental Management System

A comprehensive web-based car rental management system built with Django 4.2+, featuring user authentication, vehicle inventory management, booking system, and admin dashboard.

## 📋 Project Overview

This application provides a complete solution for managing car rental operations, including customer bookings, vehicle inventory, and administrative controls.

## 🎯 Core Features

### User Management
- Customer registration and authentication system
- Profile management
- Booking history tracking
- Secure login/logout functionality

### Vehicle Inventory Management
- Add, edit, and delete vehicles
- Vehicle categorization (Sedan, SUV, Luxury, Economy, etc.)
- Image upload for each vehicle
- Track availability status
- Rental pricing management
- Vehicle specifications (model, year, features)

### Booking System
- Date range selection with calendar interface
- Real-time availability checking
- Booking confirmation and tracking
- Booking modification and cancellation
- Price calculation based on rental duration

### Admin Dashboard
- Enhanced interface using Django Jazzmin
- Manage all vehicles and categories
- View and manage customer bookings
- User management
- Transaction history
- Analytics and reporting

### Responsive Design
- Mobile-first Bootstrap implementation
- Cross-browser compatibility
- Intuitive user interface
- Accessible design patterns

## 🛠️ Technical Stack

- **Backend Framework:** Django 4.2+
- **Language:** Python 3.10+
- **Database:** SQLite3
- **Frontend:** HTML5, CSS3, JavaScript
- **UI Framework:** Bootstrap 5
- **Admin Enhancement:** Django Jazzmin
- **Template Engine:** Django Templates

## 📊 Database Models

### User Model
- Extends Django's built-in authentication
- Additional profile fields
- Booking relationship

### Category Model
- `name`: Vehicle category name
- `description`: Category description
- `created_at`: Timestamp

### Car Model
- `name`: Vehicle name
- `model`: Car model
- `year`: Manufacturing year
- `category`: Foreign key to Category
- `rental_price_per_day`: Decimal field
- `availability`: Boolean status
- `images`: Image upload field
- `description`: Text field
- `features`: JSON field for specifications
- `created_at`, `updated_at`: Timestamps

### Booking Model
- `customer`: Foreign key to User
- `car`: Foreign key to Car
- `start_date`: Date field
- `end_date`: Date field
- `total_price`: Calculated field
- `status`: Choice field (Pending, Confirmed, Completed, Cancelled)
- `created_at`: Timestamp

## 🚀 Development Phases

### Phase 1: Project Setup (Week 1)
- Initialize Django project and app structure
- Configure database and settings
- Set up version control
- Install and configure dependencies
- Create base templates and static file structure

### Phase 2: User Authentication (Week 1)
- Implement user registration system
- Create login/logout functionality
- Build user profile pages
- Add password reset functionality
- Implement session management

### Phase 3: Vehicle Management (Week 2)
- Create Category model and CRUD operations
- Build Car model with image upload
- Implement vehicle listing pages
- Add search and filter functionality
- Create vehicle detail pages

### Phase 4: Booking System (Week 2-3)
- Develop booking form with date picker
- Implement availability checking logic
- Create booking confirmation workflow
- Build booking management interface
- Add email notifications

### Phase 5: Admin Dashboard (Week 3)
- Install and configure Django Jazzmin
- Customize admin interface
- Create custom admin actions
- Build analytics dashboard
- Implement reporting features

### Phase 6: Frontend Development (Week 3-4)
- Design responsive layouts with Bootstrap
- Create homepage and navigation
- Build vehicle browsing interface
- Implement booking flow UI
- Add user dashboard

### Phase 7: Testing & Refinement (Week 4)
- Unit testing for models and views
- Integration testing for booking flow
- UI/UX testing across devices
- Performance optimization
- Bug fixes and refinements

### Phase 8: Deployment Preparation (Week 4)
- Create deployment documentation
- Prepare production settings
- Database migration scripts
- Final code review and cleanup

## 📦 Deliverables

1. **Fully Functional Web Application**
   - All features implemented and tested
   - Responsive across devices
   - Secure and optimized

2. **Clean, Documented Code**
   - PEP8 compliant Python code
   - Comprehensive docstrings
   - Inline comments for complex logic
   - Type hints where applicable

3. **Database Migrations**
   - All migration files included
   - Migration documentation
   - Sample data fixtures

4. **Requirements File**
   - `requirements.txt` with pinned versions
   - Development dependencies separated
   - Installation instructions

5. **Documentation**
   - Setup and installation guide
   - User manual
   - Admin guide
   - API documentation (if applicable)
   - Troubleshooting guide

6. **Deployment Instructions**
   - Step-by-step deployment guide
   - Environment configuration
   - Database setup instructions
   - Static files configuration

## 🔧 Installation & Setup

```bash
# Clone the repository
git clone <repository-url>
cd django-car-rental-system

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Load sample data (optional)
python manage.py loaddata fixtures/sample_data.json

# Run development server
python manage.py runserver
```

## 📝 Project Structure

```
django-car-rental-system/
├── car_rental/              # Main project directory
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── rentals/                 # Main app
│   ├── models.py           # Database models
│   ├── views.py            # View logic
│   ├── forms.py            # Form definitions
│   ├── urls.py             # URL routing
│   └── admin.py            # Admin configuration
├── templates/              # HTML templates
│   ├── base.html
│   ├── home.html
│   └── rentals/
├── static/                 # Static files
│   ├── css/
│   ├── js/
│   └── images/
├── media/                  # User uploads
├── requirements.txt
└── manage.py
```

## 🎨 Key Features Implementation

### Availability Checking
- Real-time validation of booking dates
- Prevents double-booking
- Visual calendar with available/unavailable dates

### Price Calculation
- Automatic calculation based on rental duration
- Support for seasonal pricing (future enhancement)
- Discount codes (future enhancement)

### Image Management
- Multiple images per vehicle
- Automatic thumbnail generation
- Optimized image storage

### Security Features
- CSRF protection
- SQL injection prevention
- XSS protection
- Secure password hashing
- Session security

## 🧪 Testing

```bash
# Run all tests
python manage.py test

# Run specific app tests
python manage.py test rentals

# Coverage report
coverage run --source='.' manage.py test
coverage report
```

## 📈 Future Enhancements

- Payment gateway integration
- SMS notifications
- Advanced analytics dashboard
- Mobile app (React Native)
- Multi-language support
- Vehicle maintenance tracking
- Insurance management
- Customer reviews and ratings

## 🤝 Support & Maintenance

- 30-day post-delivery support included
- Bug fixes and minor adjustments
- Documentation updates
- Deployment assistance

## 📄 License

This project is proprietary software developed for client use.

## 👨‍💻 Developer

Professional Django developer with expertise in:
- Full-stack web development
- Database design and optimization
- RESTful API development
- Responsive UI/UX design
- Deployment and DevOps

---

**Estimated Timeline:** 4 weeks
**Budget Range:** €3,000 - €5,000 EUR
**Delivery:** Fully functional, tested, and documented application
