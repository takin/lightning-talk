# GitHub Copilot Chat Prompt

You are an expert of Go Programming language. You have more than 5 years of experiences building projects using this language. You also ver familiar with the clean architecture, both in concepts and implementation.

I want to create a golang project with these spesifications:

- The project called jagain, this is a maintenance management system
- using Golang and clean architecture pattern
- Implement the Test Driven Development Method
- the project repository is github.com/takin/jagain
- using PostgreSQL as database and Redis as cache handler
- Library that i want to use is:
  - GORM
  - Viper
  - Fiber
  - Testify
  - Go-Redis
  - Go-UUID
  - Go-Validator
  - Go-Env
  - Go-Log
Remember that i just want to use the HTTP API, so no need to create the frontend, or other channel like gRPC, etc.

I want you to prcoess it step-by step:
First, assuming that i already have create the top level folder called jagain and i am already inside these folder, now lets create all the folder structure inside it, just give me the CLI command for creating the folders and give the folder structure hierarcy. Give me the most common folder structure for rest api clean architecture pattern in Golang

Next, Lets create all the domain entity using below schema:

```sql
create table departments (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  name text not null,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table staff (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  department_id bigint references departments (id),
  name text not null,
  position text,
  phone_number text unique,
  email text unique,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table assets (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  name text not null,
  description text,
  location text,
  status text check (
    status in (
      'Available',
      'On Maintenance',
      'Need Maintenance',
      'Broken'
    )
  ) default 'Available',
  inspection_cycle_hours int,
  last_inspection_date timestamptz,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);


create table equipment (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  name text not null,
  description text,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table escalation_levels (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  name text not null,
  delay_minutes int not null,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);


create table notifications (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  title text not null,
  message text not null,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);


create table tasks (
  id bigint primary key generated always as identity,
  external_id uuid default gen_random_uuid () unique,
  staff_id bigint references staff (id),
  supervisor_id bigint references staff (id),
  asset_id bigint references assets (id),
  title text not null,
  description text,
  schedule_date timestamptz,
  completed_at timestamptz,
  target_completion_minutes int,
  status text check (
    status in (
      'Not Started',
      'In Progress',
      'Completed'
    )
  ) default 'Not Started',
  status_remarks text check (
    status_remarks in (
      'Not Started',
      'Ahead Of Time',
      'On Time',
      'Late'
    )
  ) default 'Not Started',
  priority text check (
    priority in (
      'Critical',
      'High',
      'Standard',
      'Low'
    )
  ) default 'Standard',
  escalation_level_id bigint references escalation_levels (id),
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table task_details (
  id bigint primary key generated always as identity,
  task_id bigint references tasks (id),
  detail text not null,
  notes text,
  photo_url text,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);


create table task_equipment (
  id bigint primary key generated always as identity,
  task_id bigint references tasks (id),
  equipment_id bigint references equipment (id),
  quantity int not null,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table asset_parts (
  id bigint primary key generated always as identity,
  asset_id bigint references assets (id),
  name text not null,
  description text,
  status text,
  photo_url text,
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table staff_escalation_levels (
  id bigint primary key generated always as identity,
  staff_id bigint references staff (id),
  escalation_level_id bigint references escalation_levels (id),
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

create table staff_notifications (
  id bigint primary key generated always as identity,
  staff_id bigint references staff (id),
  notification_id bigint references notifications (id),
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);
```

Give me the CLI command for creating all the domain entity file, also the struct for each entity, and the repository interface for each entity

Second, Lets create some standards
The response json structure is this:

``` json
{
  "status": "ok",
  "data":[],
  "meta": {
    "page": 1,
    "per_page": 10,
    "total_page": 5,
    "total_data": 500
  }
}
```

also, for the data there should be no record id reveal in the response, instead there is field called external_id, this is replacing the original id for all the response