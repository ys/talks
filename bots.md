# All those bots are gonna steal your job

See it as a journey

## Chatops (N.F.)

- Comes from GitHub
- Conversation driven development
- Collaboration
- visibility
- Automation
- Security

### Automation
- Page users
- Deploy your app
- Track accidents

### Visibility
- See your incidents
- see GitHub activity
- See who is deploying
- See errors in apps

### Collaboration

- Peer review for compliance
- Ack,resolve pages
- Share a framework people can build on

### Security

- 2fa through messages or push notifications
- 2 levels of authorization (chat and yubikeys)
- people see suspect actions

### bots are just software

- no need for latest Google Smartness
- smart formatting of query or NLP is not a must have! Be dumb!
- a bot is just a robot! It replaces your brain in a task that is complex or repetitive

### Important

- Have reusable patterns you could call from cli, email or sms > Go to phoneOps
- Have a Rolodex to skip mapping users and tokens
- Act on behalf of users not as a bot!
- Use a bot for funny/less important stuff and build real open source apps for the rest.

## Before

- Sentinel custom ruby bot homemade for PagerDuty
- We decide to bring that old codebase to the future, it was a mess and not really maintainable anymore
- Picking Hubot for the adoption and community
    - Coffeescript: I do Rails, I know that.
    - We will be able to bring the best of the scripts to herokai
- We were looking at the solutions that were available
- We prototyped something in Golang but dropped it to not redo the error of a custom codebase.
- We were a team of 2 working on all our projects. 
- Custom building meant we had to maintain yet another full codebase
- We also had a few other internal bots that were running on Hubot, so we thought it would be a good shot at them and replace with a single one, Maintain that more easily and get other teams to contribute.
- So what we did is rewrite all our pager duty scripts for Hubot as we have a total ownership model, the default PD script only works for an Ops team. Step one in getting custom code in.

### Our issues with Hubot

- We ended up rewriting all our scripts and not leveraging the community
- We have bad practices in testing coffee script and async code.
- Having one codebase meant we were responsible for reviewing all the people code too. And teams were not aware of which team was maintaining so > Increasing maintenance and communication cost. 
- We did not have an internal REDIS provider so we picked a Postgres Brain > Giant text blob in one column… I mean Redis is the same for the brain… That json blob is just an issue. 
- No great Postgres drivers in Node or we had issues using it.
- No rollbar easy integration
- A lot of custom code for instrumentation and monitoring
- Ended up building custom code highly tied to Hipchat.
- We were losing data sometimes and had no great visibility
- Coffeescript wasn’t our best pick, we are ruby developers in the tools team.

### Was it wrong?

- It was our best pick at the time, not a wrong choice, just did not matched with the team with time.
- We had not enough time to invest in getting great test practices and ended up with code without tests.
- All the functionalities were so different that having all that in one place made no sense for the team we built over the years.
- We are still a small team of 3 engineers and all our projects are in Rails now. We should try to standardize on that
- Callbacks, async testing, not best practices in the team meant the code was totally different depending on who wrote it.
- While we were migrating, A lot of features creeped in and we had not enough time to stabilize the core features we wanted to have.
- One good thing: We migrated from Hipchat to slack in a hurry and bringing the core functionalities even with our tight integration with hip chat took us less than a week.
- All our features were modifying the core of Hubot and not porting back to hubot as it made no sense to have it there.
- Data modeling with just a JSON blob made it tricky to have nice Structured data like we do with any relational database.
- Single process. Means we cannot scale easily and need to proxy or rely on callback hell we fighter

## Now?

- We migrated to slack definitively.
- We had time to bring core feature to different rails slack apps.
- We are removing more and more for hubot and are ready to kill it.
- We have template and best practices encapsulated into our gems.
- Clear separation of concerns, maintainability, ownership
- We can spin up new prototypes in a few days and play with them easily.
- Do we lose on not using the open sources bots?
    - Maybe but I like the flow we have now.
    - Advanced features are not portable through chat services so doing it for slack only is ok for us for now.
- Created custom rails apps per group of actions

### PagerDuty App

- Notifications for pages
- Paging user or teams
- resolving, acknowledge and raise pages

### Notifications from all the sources

- Use kafka to have a single entry point that scales.
- We don’t lose webhooks that way if app is down
- Rollbar
- Sentry
- Kernel deployment
- custom teams notifications
- Librato alerts
- Change control notifications
- and more to come

### GitHub routing of webhooks

- New issues
- New PR
- New deployment
- Commit statuses so that we get travis or Circle

### Chat deployment app

- Deploy to heroku pipelines
- Deploy to our kernel instances
- Deploy to a stage
- get deployment updates in chat.
- Deploy with yubikeys and 2fa for security
- Deployment made in the name of the user.

### Not in chat?

- Bots Checking CVE on PR
- Checking internal gems versions

## Where are we going?
- More integrations for incident handling
- Better PagerDuty integration and bringing context to pages from other apps. 
- buttons directly in chat. 
- Having a 100% accurate view of heroku platform from chat
- Onboarding through chat a la 18f ??
- Security via 2fa on your phone thanks to SalesforceAuth or duo
- Expose chat actions in normal Web UI and CLI for when chat is down
