## Room Booking Application

This application provided the basics of building web application with Go via CRUD, Database queries and the use of Session.

Visit [Website](https://li1450-170.members.linode.com/)

### This application allows users

- Search room by dates
- Make reservation
- Sends an email to the guest for making a reservation

### The Admin of the app

- Can see all reservations
- Process reservations
- Approved reservation
- Block rooms for renovation
- Receives an email when a reservation is made

### Tech stack used

- Styled with [Bootstrap 5](https://getbootstrap.com/)
- Built in Go version [1.17](https://golang.org/dl/)
- Uses the [chi router](https://github.com/go-chi/chi/v5)
- Uses [alex edwards SCS](https://github.com/alexedwards/scs/v2) Session Manager
- Uses [nosurf](https://github.com/justinas/nosurf) for CSRF protection
- Date Picker [VanillaJS datepicker](https://mymth.github.io/vanillajs-datepicker/#/)
- Modal windows [SweetAlert2](https://sweetalert2.github.io/)
- Flash messages [Notie](https://github.com/jaredreich/notie)
- Email Template used is [Foundation](https://get.foundation/emails/getting-started.html)
- Email address validator is [govalidator](https://github.com/asaskevich/govalidator)
- Database adapter used is [pgx](https://github.com/jackc/pgx)
- Localhost email catcher is [MailHog](https://github.com/mailhog/MailHog)
- Admin dashboard template is [RoyalUI](https://github.com/BootstrapDash/RoyalUI-Free-Bootstrap-Admin-Template)
- [Simple Tables](https://github.com/fiduswriter/Simple-DataTables)
- Simple database migration by [Soda](https://gobuffalo.io/en/docs/db/toolbox/)
- Hosted on [Linode](https://www.linode.com/)
- Server used is [Caddy](https://caddyserver.com/)
