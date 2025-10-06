# Movie-Ticket-Booking-System (Django + DRF)

## Setup (development) 
1. Clone repo.
2. Create and activate virtualenv:
   python -m venv venv
   venv\Scripts\activate # Windows
   
3. Install: pip install -r requirements.txt

4. Create `.env` file in project root (optional, or use defaults):
   SECRET_KEY=change-me
   DEBUG=True DB_ENGINE=django.db.backends.sqlite3
   DB_NAME=db.sqlite3

5. Run migrations:
   python manage.py migrate

6. Create superuser:
   python manage.py createsuperuser

7. Run server: python manage.py runserver

## API 
- Swagger docs: `http://127.0.0.1:8000/swagger/`
- Signup: POST `/api/signup/` => {username, password, email?}
- Login: POST `/api/login/` => returns `access` and `refresh` tokens
- Use header: `Authorization: Bearer <access_token>`
- List movies: GET `/api/movies/`
- List shows for movie: GET `/api/movies/<movie_id>/shows/`
- Book a seat: POST `/api/shows/<show_id>/book/` with JSON `{"seat_number": 12}`
- List my bookings: GET `/api/my-bookings/`
- Cancel booking: POST `/api/bookings/<booking_id>/cancel/`

## Notes on Business Rules
- Double booking prevented with `unique_together` (show, seat_number) + transaction checks.
- Overbooking prevented by counting booked seats in a transaction.
- Cancelled bookings free the seat (status becomes `cancelled`).
- Security: only owner or admin can cancel a booking.

## Bonus 
- Concurrency protection uses `select_for_update()` + `transaction.atomic()`. For high concurrency, consider database-level constraints and retry/backoff logic at the API layer.
