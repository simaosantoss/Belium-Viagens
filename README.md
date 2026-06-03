# ✈️ BeLIUM Viagens

![Database](https://img.shields.io/badge/database-MySQL-4479A1)
![Language](https://img.shields.io/badge/language-SQL-336791)
![Course](https://img.shields.io/badge/course-Databases-orange)

> A relational database system for preserving, organizing, and analyzing CeSIUM travel history.

**BeLIUM Viagens** is a MySQL database project developed for the **Databases** course, a 2nd-year, 1st-semester course in the Software Engineering degree at the University of Minho.

The project models a fictional travel archive for **CeSIUM** (*Centro de Estudantes de Engenharia Informática da Universidade do Minho*), the Computer Science Students' Association at the University of Minho, where members can register trips, stops, sponsors, feedback, reactions, photos, and financial support.

The submitted project report is available in [`docs/thesis/belium-thesis.pdf`](./docs/thesis/belium-thesis.pdf).

The report and the SQL identifiers are written in Portuguese because this was an academic project developed and evaluated in Portuguese.

---

## 📌 At a Glance

| Area | Implemented |
|---|---:|
| Relational tables | 11 |
| Database roles | 3 |
| Stored procedures | 19 |
| SQL functions | 3 |
| Analytical views | 3 |
| Performance indexes | 4 |
| Trigger-based automation | 1 trigger |
| Prepared statement examples | 7 |
| Demo population calls | 152 |
| Project report | 102 pages |

---

## ✨ What This Project Demonstrates

This project follows the complete database development lifecycle, from conceptual modeling to SQL implementation.

| Topic | What was implemented |
|---|---|
| Conceptual modeling | Entity-Relationship model built from the project requirements |
| Logical modeling | Relational schema designed in MySQL Workbench |
| Normalization | Schema refined up to Third Normal Form (3NF) |
| Referential integrity | Primary keys, composite keys, foreign keys, and relationship tables |
| Domain constraints | `CHECK` constraints, `ENUM` domains, required fields, and unique values |
| Database logic | Stored procedures for insertion, authentication, trip filtering, reactions, and password changes |
| Analytical queries | Views, functions, prepared statements, and filtered query procedures |
| Access control | Three-role permission model using MySQL users and grants |
| Automation | Trigger that keeps the number of trip stops synchronized |
| Optimization | Indexes for date, location, trip-detail, and reaction filters |

---

## 🧩 Domain Model

The database stores trips as historical records composed of stops, transport methods, photos, sponsors, publications, and user reactions.

<p align="center">
  <img src="./docs/er_diagram.png" alt="Entity-Relationship diagram" width="900">
</p>

---

## 🗃️ Core Entities

| Entity | Purpose |
|---|---|
| `Utilizador` | Stores platform users with name, phone number, email, and password |
| `Sócio` | Extends users that belong to CeSIUM, including member number and status |
| `Viagem` | Stores trip dates, number of stops, participants, cost, and objective |
| `Paragem` | Represents each city/country stop in a trip itinerary |
| `Deslocamento` | Stores the transport methods used in a trip |
| `Foto` | Associates photos with a specific stop of a specific trip |
| `Patrocinador` | Stores sponsoring organizations |
| `Rel_Viagem_Patrocinador` | Connects sponsors to trips and stores sponsored value |
| `Motivo` | Explains why a sponsor supported a trip |
| `Rel_Utilizador_Viagem` | Stores user reactions to viewed trips |
| `Rel_Sócio_Viagem` | Stores member publications, ratings, and comments about trips |

---

## 🗺️ Logical Schema

The conceptual model was transformed into a normalized relational schema designed for MySQL.

<p align="center">
  <img src="./docs/logical_schema.png" alt="Logical relational schema" width="900">
</p>

---

## 🔐 Security Model

The system defines a role-based permission model with three MySQL users.

| Role | Database user | Main permissions |
|---|---|---|
| Public user | `UTILIZADOR` | Read public trip/member data, react to trips, update own password |
| Member | `SOCIO` | All public-user permissions, plus trip insertion/update/removal and member publications |
| Administrator | `ADMIN` | Full database privileges, including grant management |

This separation was designed to preserve personal data, protect structural tables from unauthorized writes, and mirror realistic responsibility boundaries inside an academic association.

---

## 🧠 Database Logic

### Stored Procedures

The stored procedure layer centralizes the main write and interaction workflows.

| Group | Procedures |
|---|---|
| Insertion API | `InserirUtilizador`, `InserirSocio`, `InserirViagem`, `InserirParagem`, `InserirFoto`, `InserirPatrocinador`, and relationship insertions |
| Authentication | `AutentificaUtilizador` |
| Account management | `MudarPassword` |
| Trip interaction | `InserirVisualização`, `AtualizarVisualização`, `RemoveVisualização` |
| Search and filtering | `FiltrarViagensPorLocalização`, `FiltrarViagensPorDetalhes`, `FiltrarViagensPorReações` |

### Functions

| Function | Purpose |
|---|---|
| `CalcularInvestimentoTotal` | Calculates the total amount sponsored by one sponsor |
| `CalcularInvestimentoMédio` | Calculates average investment per sponsored trip |
| `CalcularPercentagemPositiva` | Calculates the positive-reaction percentage of a trip |

### Views

| View | Purpose |
|---|---|
| `VisualizarInvestimentoTotal` | Lists total investment by sponsor |
| `VisualizarNrPublicações` | Ranks members by number of trip publications |
| `VisualizarViagensPopulares` | Shows the ten trips with the most views/reactions |

### Trigger

| Trigger | Behavior |
|---|---|
| `AtualizaNrParagens` | Automatically increments `Viagem.Nr_Paragens` whenever a new stop is inserted |

---

## 📁 Repository Structure

```text
Belium-Viagens/
├── README.md
├── src/
│   ├── Database.sql              Creates the BeLIUM_Viagens database
│   ├── Tables.sql                Defines tables, keys, relationships, and constraints
│   └── Users.sql                 Creates database users and grants permissions
├── adv/
│   ├── Insersion-Procedures.sql  Stored procedures for controlled inserts
│   ├── Other-Procedures.sql      Authentication, password, reactions, and filters
│   ├── Functions.sql             Analytical SQL functions
│   ├── Views.sql                 Analytical views
│   ├── Triggers.sql              Trigger-based maintenance logic
│   ├── Indexes.sql               Query optimization indexes
│   ├── Prepared-Statements.sql   Prepared query examples for requirements
│   └── Population.sql            Demo dataset
└── docs/
    ├── er_diagram.png            Entity-Relationship diagram
    ├── logical_schema.png        Logical relational schema
    └── thesis/
        └── belium-thesis.pdf     Full project report
```

---

## ⚙️ Requirements

- MySQL Server 8.0 or compatible
- MySQL Workbench or another MySQL GUI/client connected to the server
- UTF-8 compatible editor or MySQL client

Some table, column, procedure, and function names use Portuguese accents, such as `Sócio`, `Telemóvel`, `País`, `Reação`, and `CalcularInvestimentoMédio`. Use a UTF-8 environment when running or editing the scripts.

---

## 🚀 Running the Project in MySQL

The project is meant to be opened, executed, tested, and inspected in **MySQL Workbench** or another MySQL database client.

Open and execute the SQL files in this order:

| Step | File | Purpose |
|---:|---|---|
| 1 | `src/Database.sql` | Recreates the `BeLIUM_Viagens` database |
| 2 | `src/Tables.sql` | Creates all tables, keys, relationships, and constraints |
| 3 | `adv/Functions.sql` | Creates analytical SQL functions |
| 4 | `adv/Insersion-Procedures.sql` | Creates insertion procedures |
| 5 | `adv/Other-Procedures.sql` | Creates authentication, password, reaction, and filter procedures |
| 6 | `adv/Triggers.sql` | Creates trigger-based maintenance logic |
| 7 | `adv/Views.sql` | Creates analytical views |
| 8 | `adv/Indexes.sql` | Creates performance-oriented indexes |
| 9 | `adv/Population.sql` | Loads the demo dataset |
| 10 | `src/Users.sql` | Creates database users and grants permissions |

`src/Users.sql` should be executed with an account that can create users and grant privileges.

Prepared statement examples can be executed after the schema and demo data are loaded:

| Optional file | Purpose |
|---|---|
| `adv/Prepared-Statements.sql` | Runs prepared query examples associated with project requirements |

---

## 🔍 Example Queries

### Popular Trips

```sql
SELECT *
FROM VisualizarViagensPopulares;
```

### Trips by Location

```sql
CALL FiltrarViagensPorLocalização('Portugal', NULL);
```

### Trips by Date, Cost, and Objective

```sql
CALL FiltrarViagensPorDetalhes(
    '2025-01-01',
    '2025-12-31',
    NULL,
    1300.00,
    'Ambos'
);
```

### Trips with Strong Positive Feedback

```sql
CALL FiltrarViagensPorReações(80);
```

### Sponsor Investment

```sql
SELECT
    P.ID,
    P.Nome,
    CalcularInvestimentoTotal(P.ID) AS total_investment,
    CalcularInvestimentoMédio(P.ID) AS average_investment
FROM Patrocinador AS P;
```

### Change a User Password

```sql
CALL MudarPassword(
    'user@example.com',
    'old-password',
    'new-password'
);
```

---

## 📊 Demo Dataset

The population script creates a small but connected dataset that exercises the full schema.

| Data type | Amount |
|---|---:|
| Users | 20 |
| CeSIUM members | 10 |
| Trips | 5 |
| Transport records | 11 |
| Stops | 16 |
| Photos | 16 |
| Member trip publications | 17 |
| Sponsors | 3 |
| Sponsorship records | 2 |
| Sponsorship motives | 2 |
| User trip reactions | 50 |

The demo data includes trips across Portugal and Spain, different objectives (`Pedagógico`, `Recreativo`, and `Ambos`), member ratings, public reactions, sponsorship values, and itinerary photos.

---

## ⚡ Indexes and Query Support

The project adds targeted indexes for the requirements that naturally involve filtering or ranking.

| Index | Columns | Supports |
|---|---|---|
| `IDX_Paragem_Data_Chegada` | `Paragem(Data_Chegada)` | Ordering/filtering stops by arrival date |
| `IDX_Paragem_Localização` | `Paragem(País, Cidade)` | Filtering trips by country and city |
| `IDX_Viagem_Detalhes` | `Viagem(Data_Início, Data_Término, Custo, Objetivo)` | Filtering trips by date, cost, and objective |
| `IDX_Rel_Utilizador_Viagem_Reação` | `Rel_Utilizador_Viagem(Reação)` | Positive/negative reaction analysis |

---

## ✅ Validation Notes

The schema enforces several data-quality rules directly in the database:

- emails must follow a basic `name@domain` pattern;
- phone numbers must start with `+`;
- trip start date must be earlier than trip end date;
- stop arrival date must be earlier than stop departure date;
- trip cost and sponsorship value must be positive;
- photo paths must follow the expected project folder format;
- stop insertion rejects overlapping stop intervals for the same trip;
- foreign keys protect relationship consistency between users, members, trips, sponsors, stops, and photos.

---

## 📚 Documentation

| File | Description |
|---|---|
| [`docs/thesis/belium-thesis.pdf`](./docs/thesis/belium-thesis.pdf) | Full submitted project report |
| [`docs/er_diagram.png`](./docs/er_diagram.png) | Conceptual Entity-Relationship diagram |
| [`docs/logical_schema.png`](./docs/logical_schema.png) | Logical relational schema generated from the MySQL model |

---

## ⚠️ Notes and Limitations

- The project is a database prototype, not a full web or desktop application.
- The dataset is fictional and was created for academic validation.
- Passwords in the demo data are plain text because the focus of the assignment was relational modeling, permissions, procedures, and SQL logic.
- Identifiers and runtime messages are mostly in Portuguese to match the original requirement specification.
- The current model is optimized for the required academic queries, not for production-scale travel management.

---

## 👥 Authors

| Name | GitHub |
|---|---|
| [Bruno Magalhães](https://github.com/Brumag777) | `@Brumag777` |
| [Diogo Azevedo](https://github.com/azevedodiogo) | `@azevedodiogo` |
| [Simão Santos](https://github.com/simaosantoss) | `@simaosantoss` |
| [Vera Almeida](https://github.com/veralmeida23) | `@veralmeida23` |
