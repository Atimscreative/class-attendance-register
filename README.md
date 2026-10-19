# Class attendance register

A register for a class. The tutor marks who came to each session. Each
learner sees their own record and their attendance rate. A wrong mark
can be corrected, and the learner can see that it was. At the end of
term, the tutor downloads the term as a file that someone in the office
can rely on.

## What finished looks like

Anyone can open the app and:

- create an account, and still be signed in when they come back
- as a tutor, set up a class, add its learners and say which days it
  meets
- mark each learner present, late, absent or excused
- correct a wrong mark later, in a way the learner can see
- as a learner, see their own record and nobody else's, with a rate
  that says what it counted
- as a tutor, download the term as a file that states its dates,
  matches the screen it came from, and does not change after it has
  been handed in

Finished means it is live on the internet, not just on your laptop. A
tutor you have never met can open it on a Monday morning and take the
register.

## The road map

Eight sprints. Each sprint delivers one part of the list above. They
must be done in order, because each one builds on the one before.

| # | Sprint | What it delivers |
|---|---|---|
| 1 | Getting in | People create accounts and stay signed in. Tutor accounts cannot be created through the public sign-up |
| 2 | A class, and the days it meets | A tutor creates a class with learners, and the term appears as a list of dated sessions |
| 3 | Four ways to be missing | A tutor marks each learner present, late, absent or excused |
| 4 | A number somebody can defend | Each learner has an attendance rate that shows what it counted |
| 5 | Their own record, and nobody else's | A learner sees their own attendance and cannot see anybody else's |
| 6 | Marked absent, was there | A tutor corrects a wrong mark, and the learner sees the correction |
| 7 | The term as a file | A term downloads as a file that matches the screen |
| 8 | Where a school reaches it | The app is live on the internet at an address a school can give out |

## The hard parts

**The file.** A grid of ticks is easy. The file is harder, because
someone in an office makes decisions from it. It must say which dates
it covers. It must match what the screen showed when it was made. If a
mark is corrected the week after, the file that was already handed in
must not change. And someone who was not there must still be able to
read it a year later.

**The number.** Present, late, absent and excused are four different
marks. An attendance rate is one number. Before you can work it out,
you have to decide what each mark does to it. For example, does an
excused absence count against the learner? If you do not decide on
purpose, nobody can explain the number to the learner it is about.

## Working on it

The sprints, their briefs and their tickets are in Blacksmith
Experience. Start with sprint 1. Each sprint opens when the one before
it is finished.

Read the documents attached to a sprint before you start its first
ticket. They explain the choices you will make and the options you are
choosing between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
