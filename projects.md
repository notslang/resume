# Projects

This is a list of projects I have worked on, organized by the type of project. This includes personal projects, as well as projects I've done while working at Carrot and RokkinCat.

Note: I did not create UI designs for any of these. Designs were provided either by a separate design team or by the client

## Elixir

### API Server (Driver's Seat Cooperative)

> Data analytics and reporting platform for Driver's Seat Cooperative (https://driversseat.co)

Driver's Seat Cooperative (DSC) is a mobile application for gig workers (Uber, Lyft, GrubHub, etc). The purpose of the application is to help workers find the best strategy for earning money on these types of gig applications. It combines earnings and trip data pulled from gig apps with GPS point data collected by the DSC app itself to help gig workers find the most profitable locations, types of jobs, and shift times.

I was responsible for building the backend and data analytics pipeline while other RokkinCat team members handled the frontend. For this I used Elixir, PostgreSQL, and PostGIS. The application is hosted on Heroku, but a migration to Fly.io is planned.

### Conference Event Web Application (Snap-on Inc)

> Snap-on Inc conference & livestream platform

Snap-on is a manufacturer and marketer of high-end tools and equipment for professional technicians. They sell their tools to end-users through a network of franchise owners. Prior to COVID, Snap-on hosted large conferences for their franchise owners to learn about new tools and place orders for the upcoming year. In mid-2020, Snap-on requested that RokkinCat build a custom web application to host franchisee virtual events, due to COVID restrictions that made physical events impossible.

I built the bulk of the site in Elixir with the [Phoenix Framework](https://www.phoenixframework.org). [Stimulus](https://stimulus.hotwired.dev/) was used for the frontend JavaScript. I also maintained the site for more than 2 years. It went through multiple redesigns to cater to different events, including the 100th anniversary "Live from the Forge" celebration in 2020, the "Own Every Moment" campaign in 2021, and the "Marketing Toolbox" resource site that's used to this day.

The site featured multiple coordinated live streams and product reveals, regional and per-franchise personalization, and searchable product videos / marketing materials.

The first version of the site had a content management system implemented with [Elixir & Torch](https://github.com/mojotech/torch), including a tool to sync video metadata with Snap-on's Vimeo channel, and an internationalization tool. Later versions of the site used [Sanity CMS](https://www.sanity.io/) to edit content which was pulled down and rendered into templates by the Elixir backend.

This project included:

- Internationalization for US and French Canadian markets with [Gettext](https://hexdocs.pm/gettext/Gettext.html)
- Performance testing with multiple Heroku dyno types using [k6](https://k6.io/) for load testing
- Integration with the internal Snap-on franchisee database and SSO system, to only allow franchisees in certain regions to access the app
- Reporting system for tracking logins for each franchisee for an internal contest
- A video analytics tool for Snap-on's marketing team to track the performance of each of their product videos
- ETS based caching system to handle the load from thousands of concurrent users accessing the site during events

### Estimator (RokkinCat Internal Tooling)

At RokkinCat we frequently produce cost & time estimates to tell our clients how long a project will take and what budget is required. Estimator is a piece of internal tooling to help with that process. With estimator, an engineer will break down a product or feature into a set of "stories" and then give a high, expected, and low number of hours for each story. This is known as PERT estimation. Using these data points, a probability distribution is calculated for the total time that the project will take across all features and an estimate is selected, usually in the 80th-95th percentile, depending on risk tolerance for the project.

I worked with Jason to implement this system in Elixir & Ember.

### Frontend and CMS (Region.co.uk)

> Hyperlocal news site and custom CMS for Region.co.uk

> Region was an early-stage startup focused on delivering high-quality hyperlocal journalism in the UK. Their site has customers subscribe to news by post code and delivers stories around all municipalities that post code is contained within. The site is an Elixir Phoenix API that serves a React single-page frontend. Sean was largely responsible for the full frontend application, working closely with the client who is also a graphic designer.

Region was an experimental hyper-local news site that existed from mid-2018 until the end of 2019. By the end, the site contained around 300 stories, targeted at specific areas in the UK. Areas were defined by groups of postcodes and stories were written as sets of easily digestible slides with a short text on each and full-screen images.

Founders Oliver and Ash asked RokkinCat to build this site, and I was responsible for the frontend and most of the backend.

Elixir was used for the backend server, which rendered stories with EEx templates and provided a simple admin interface with Torch. React.js was used for the rich text editor and other parts of the CMS. Vanilla JavaScript was used for the frontend. Styles were written with Sass. We also integrated with Google Cloud Storage and [Imgix](https://imgix.com/) for storing and serving images.

After building out the first version of the site, I designed the architecture for an updated version that would include monetization through an advertisement system, a new way of presenting stories that didn't use the slideshow format, and structured evergreen content types for organizations and notible people in the UK. I estimated the budget for this new version, and pitched it, but the founders were never were able to raise funds to implement it.

### SwayDM

> Backend for paid messaging platform SwayDM (https://sway.dm)

Built administrative UI, search functionality, and backend updates for sending messages to multiple recipients.

### Beyond The Bell

Elixir application hooked up to AirTable for content management. Provided an API for their mobile application.

mid-2019, pretty short project.

## Server Management

### SAFIO Solutions

[SAFIO](https://safiosolutions.com/) is a provider of demand forecasting software. They help businesses predict future sales volume throughout the year and plan purchasing & manufacturing volume based on those models.

In early 2022 I was called in to review the codebase, fix performance issues, and improve the deployment process for their application. The application is hosted on AWS and consists of a Vue frontend that communicates with a Node.js / TypeScript backend. The database is a multi-tenant PostgreSQL setup running on RDS.

Prior to me coming on the project, new updates to the application were deployed by manually copying code over to the production server and compiling the TypeScript & Vue files on the live server.

In order to improve the application, I:

- Wrote an automated deployment script that builds the frontend and backend applications with [GitHub Actions](https://github.com/features/actions), packages them as Docker containers, and saves them to the Amazon Elastic Container Repository (ECR). With this setup images can be tracked through the staging / quality assurance pipeline. Also, code is no longer compiled on the production server, resulting in less downtime and less risky deployments.
- Created an [Ansible](https://www.ansible.com/) playbook to manage the configuration of QA, staging, production, and tenant-specific servers. Secrets for each environment were managed with Ansible Vault. GitHub Actions was used to automatically test and deploy configuration changes to the inventory of servers, each time an update was made.
- Optimized nightly forecasting jobs to run concurrently, cutting the 8 hour runtime in half.

### WMSE Radio 91.7FM

> Radio WMSE 91.7’s website and show archival system (https://wmse.org)

WMSE is a local Milwaukee radio station that live-streams music through their website, [wmse.org](https://www.wmse.org/). Their site is hosted with [NGINX](https://www.nginx.com/) running on CentOS Linux box in an Azure instance provided by the Milwaukee School of Engineering (MSOE).

In mid-2020 was called in to migrate their website to a new server, fix performance issues, and automate deployments for new versions of the site. Prior to me coming on the project, the site was configured with ad-hoc changes to config files on the production server and no documentation was written.

In order to make the site more manageable, I:

- Wrote an [Ansible](https://www.ansible.com/) playbook to build new instances of the site from scratch and manage configuration changes in the future. With this setup all the config files are documented in a GitHub repo and a new instance of the site can be spun up in under an hour.
- Setup Cloudflare as a DNS host and basic caching layer
- Setup a staging server with an identical configuration to test changes before they are deployed to the main site
- Configured NGINX to cache certain expensive API endpoints
- Enabled process monitoring and restarting of their show archival service using systemd

## Ruby

### Manomet GSC

late-2019

Updated an outdated Ruby on Rails + PostgreSQL app used by a company that audits the environmental impact of grocery stores. Fixed lots of broken features.

### CTEC

mid-2019

Transitioned a legacy Ruby on Rails + MongoDB application used by a construction company from DigitalOcean to Heroku to make maintenance easier

Also upgraded a ton of outdated Ruby code.

## Node.js

### Radio Milwaukee Playlist Updater

> Radio Milwaukee 88.9’s online playlist software (https://radiomilwaukee.org)

### Roots (Carrot Creative Open Source Tooling)

A static site compiler for building simple, beautiful, and efficient products for the web.

## Frontend Development

### Scout Guest Check-in Site (Frontdesk)

[Frontdesk](https://www.stayfrontdesk.com/) is a company that manages short term rental units using a custom software platform, written in Python.

In mid-2021, I was called in to build a check-in site called Scout where guests could enter their booking confirmation code and receive details about their stay. For example, where to park, what the WiFi password is, how to get their key to the unit, etc. I built this site with Python / Django and integrated it into their existing Python-based back end to automatically pull details about each unit from the database.

This was my first time using Django, and I thought it was a pretty good CMS. I don't maintain this tool anymore, but it is still used to this day.

### Levr Solutions Marketing Site (Frontdesk)

In addition to managing short-term rentals, Frontdesk also sells their rental management software platform under the brand name Levr.

In late 2021, I was tasked with building the marketing site for Levr, [levrsolutions.com](https://levrsolutions.com/).

Next.js, deployed to Heroku.

Designs were finalized about a week before launch, and assets were still being produced days before launch, but I still got it done on time.

### Snapifeye Ionic App (Northwestern Mutual)

> Ionic app for Northwestern Mutual called Snapifeye

### Carrot Brand Website (Carrot Creative)


## ArtBeyond.org

[ArtBeyond](http://artbeyond.org/) is a gallery website launched in early 2013 that I created in collaboration with Chaz Southard from [CCS Design House](http://www.ccsdesignhouse.com/). The site is made to promote an art gallery which is being used to raise funds to help fuel spinal cord research at MIT.

All content on the site is managed through a customized WordPress backend, that holds tagged imagery from 12 different artists. The site dynamically re-organizes itself to fit to screens as small as 300px, making it very mobile friendly. Also, it includes a guest-book feature that allows people visiting the gallery to share comments on their experience.

ArtBeyond and CCS Design House no longer exist.
