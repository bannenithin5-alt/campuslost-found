# CampusLost – College Lost & Found

Spring Boot + Thymeleaf + MySQL web application (the original single-file HTML app, rebuilt as a full-stack project).

| Layer     | Technology                              |
|-----------|-----------------------------------------|
| Backend   | Java 17+, Spring Boot 3.3               |
| Frontend  | Thymeleaf, HTML5, CSS3, JavaScript      |
| Database  | MySQL 8                                 |
| ORM       | Spring Data JPA / Hibernate             |
| Security  | Spring Security (BCrypt, form login, CSRF, roles) |
| Build     | Maven                                   |
| UI        | Responsive (desktop / tablet / mobile)  |

## Run it

1. Install **JDK 17+**, **Maven 3.9+** and **MySQL 8** (running on port 3306).
2. Set your MySQL login (or edit `src/main/resources/application.properties`):

   ```bash
   # Linux / macOS
   export DB_USERNAME=root
   export DB_PASSWORD=your_mysql_password
   # Windows (PowerShell)
   $env:DB_USERNAME="root"; $env:DB_PASSWORD="your_mysql_password"
   ```
3. Start:

   ```bash
   mvn spring-boot:run
   ```
4. Open <http://localhost:8080>

The database `campuslost` and all tables are created automatically on first start.

## Default accounts (created on first run)

| Role    | Email                    | Password   |
|---------|--------------------------|------------|
| Admin   | admin@campuslost.com     | admin123   |
| Student | student@campuslost.com   | student123 |

**Change the admin password** in `application.properties` before the first run if this is deployed anywhere public.
Set `campuslost.seed-demo-data=false` to skip the demo student and the two sample reports.

## Features

**Students**
- Register / login / logout
- Report lost or found items (with photo upload)
- Browse, filter (All / Lost / Found / Recovered) and search the public feed
- Public report page (phone, email, full ID and identifying marks are hidden)
- Claim a found item by describing something only the owner would know
- "My Activity" page: own reports and claim status

**Admin** (`/admin`)
- Dashboard with live stats and recent activity
- All reports: search, filter, view full private details, mark recovered, delete
- Smart Matches: lost <-> found scoring (name, category, colour, location, date, description) with one-click "Mark Both Recovered"
- Claims: approve (marks the item recovered, auto-rejects other pending claims) or reject
- Users list

## Project structure

```
src/main/java/com/campuslost/
  CampusLostApplication.java
  config/       SecurityConfig, WebConfig (uploads), DataSeeder
  controller/   Home, Auth, Report, Admin
  dto/          ReportForm, RegisterForm, Stats, MatchResult
  entity/       User, Report, Claim (+ enums)
  repository/   UserRepository, ReportRepository, ClaimRepository
  service/      UserService, ReportService, ClaimService, MatchService, FileStorageService
src/main/resources/
  templates/    index, login, register, report-form, report-detail, my, error, fragments, admin/*
  static/       css/style.css, js/app.js
  application.properties
```

## Database tables

`users`, `reports`, `claims` (created by Hibernate). Uploaded photos are saved in the `uploads/` folder next to the app.
