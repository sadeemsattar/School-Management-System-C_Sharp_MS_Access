# School Management System (C# WinForms + MS Access)

A desktop school management application built as a university coursework project (third semester, 2020) for Human–Computer Interaction and Software Requirements Engineering courses. It is a C# Windows Forms app backed by a Microsoft Access database, written to practice object-oriented design (see `src/STUDENT MANAGEMENT SYSTEM/ClassDiagram1.cd` and the `person` / `student` / `teacher` / `admin` class hierarchy).

**This is an archival student project.** It is kept as a record of coursework, together with the original reports in `docs/`. It is Windows-only and has not been modernized.

## Features

Four user roles log in with an ID, password, and role selection:

- **Admin** — register students and teachers, view their details, post announcements (targeted at teachers, students, accounts, or everyone), set and view class/teacher timetables.
- **Accounts** — view student fee records, generate and submit fee payments, post announcements to specific students.
- **Teacher** — view own details and timetable, mark and update student attendance, enter and update quiz/mid-term/final marks, post and view announcements.
- **Student** — view own profile, attendance, marks, class timetable, and announcements; view and print a fee challan and check fee status.

## Tech stack

| Component | Choice |
|---|---|
| Language / UI | C# on .NET Framework 4.0 (Client Profile), Windows Forms |
| Database | Microsoft Access (`.mdb`), via `System.Data.OleDb` with the `Microsoft.Jet.OLEDB.4.0` provider |
| IDE | Visual Studio (solution in 2010 format; last opened with VS 2019) |
| Platform | Windows only, compiled x86 (the Jet 4.0 provider is 32-bit only) |

## Repository layout

```
docs/   Original coursework deliverables: HCI proposal, hypothesis,
        design rationale, prototype and functionality reports,
        Shneiderman/Norman heuristics analysis, SRS, use-case PDF
src/    Visual Studio solution
  STUDENT MANAGEMENT SYSTEM.sln
  STUDENT MANAGEMENT SYSTEM/         WinForms project (entry point: Program.cs -> Form3 login)
    Information123.mdb               main demo database (users, records)
    Information1.mdb                 secondary demo database
    Connection/Connection.txt.example  template for the runtime DB config
```

## Running it (Windows + Visual Studio)

This project was not built or tested as part of the repo cleanup (macOS environment); the steps below describe the intended usage.

1. Open `src/STUDENT MANAGEMENT SYSTEM.sln` in Visual Studio on Windows. You will need the .NET Framework 4.0 targeting pack (or let VS retarget to a later 4.x framework).
2. Build in Debug/x86. The `.mdb` databases are copied next to the executable automatically.
3. The app reads its database connection string from a plain-text file `Connection\Connection.txt` in the working directory of the executable (e.g. `bin\Debug\Connection\Connection.txt`). Create that folder and file from `Connection/Connection.txt.example`, pointing `Data Source` at the built copy of `Information123.mdb`. Example:

   ```
   Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\...\bin\Debug\Information123.mdb
   ```

4. Run. Log in with credentials from the demo database (the login table holds sample accounts, including an admin account created for the course demo).

The app also writes small session files (`Connection\atdu.txt`, `Connection\ttdp.txt`) to remember the logged-in user; it creates and deletes these itself.

## Known limitations

Kept as-is to preserve the project's original state; do not use this as a template for production code.

- **Windows/32-bit only.** The Jet 4.0 OLE DB provider only exists as a 32-bit Windows component; the project must stay x86.
- **SQL is built by string concatenation** (no parameterized queries), so the app is SQL-injectable by design standards of a first coding project.
- **Passwords are stored in plain text** in the Access database and compared in plain text at login.
- **Configuration is a text file, not app.config.** `app.config` declares dataset connection strings (including one for a `login123.mdb` that is not present in the repo), but the runtime code paths all read `Connection\Connection.txt` instead.
- **Forms are mostly numbered** (`Form1`–`Form19`) rather than named after their function.
- The demo databases contain fabricated sample records (dummy names, emails, and credentials) from the course demo.

## Documentation

The `docs/` folder contains the original submitted coursework: project proposal, hypothesis, functionalities list, design rationale, prototype report, a Shneiderman & Norman usability-heuristics analysis, the SRS, and a use-case diagram PDF. They describe the system as designed and are kept unmodified for context.
