# dbt_fundamentals

I'm learning how to use dbt with BigQuery so I can apply that knowledge wherever we end up working. It seems like a good DWH interface tool to know, and allows me to solidify concepts of testing in data ops.

## Sia's Set Up Steps Notes

My notes for how I went from never having used dbt to setting up my first project. This was just as I went. Coming back to update with more details and links. A lot of this was helped by dbt's setting up docs page, but there were some assumptions made about how to navigate and wanted to break it down even more than it was on the tutorial (adding links after)

### Objective

- You have a data warehouse and some queries to run to generate information
- You want to bring in the data from the warehouse, transform the data, and test to see that you did it correctly and made the correct assumptions based on agreed upon "rules".
- DBT allows you to do the second thing in a streamlined abstracted layer without having to make too many custom scripts in your org.
- I'm learning this because I've only ever done this manually or with a bunch of custom files :clown: :smile:

### BigQuery

- set up a cloud GCP account, three month free trial
- create a cloud project (keep in mind the project id)
- create a user who is going to be accessing big query as an admin
- create a credential for the user you created and save the keys as a JSON
- download the JSON and save it in the path of profiles where dbt is installed (hold on, just download and wait, see below)

### DBT CLI

- go to terminal or the code environment
- I created a `venv` so it is all self contained
- in the `venv`, pip install dbt-bigquery because it will allow you to connect your DWH big query data credentials to dbt
- once installed, run `dbt --version` to make sure everything you want is installed
- if all is set (you should see `dbt-core` and `dbt-bigquery`), then its time to initiate the project
- run `dbt init jaffle_shop` to create a starter for the project (git repo notes to be added after)
- once done, update the `profiles.yml` file with your credentials for the `jaffle_shop` profile (this file is generated when you run init but you need to add credentials)

### DBT x Google BigQuery

- for each project, there will be a profile with the name of the project and credentials to warehouse it is using
- the dbt profile yml has the name of the project, the general .dbt profiles contains all the profiles and their settings so that's how they're connected
- **remember the json file we set up and downloaded?**. Take that and save it in the `.dbt` user folder on your computer
- Now we update credentials. When updating credentials in the `profiles.yml` file, first you want to copy this chunk below in under the `jaffle_shop` branch (so go one branch in).

        ```yml
        target: dev
        outputs:
            dev:
            type: bigquery
            method: service-account
            keyfile: /Users/your_pc_or_mac_user/.dbt/dbt-learn-cred.json # replace this with the full path to your keyfile
            project: dbt-learn-fundamentals # Replace this with your project id
            dataset: dbt_sia # Replace this with dbt_your_name, e.g. dbt_bob or your schema name, this was an example from the dbt tutorial
            threads: 1
            timeout_seconds: 300
            location: US
            priority: interactive
        ```
- After adding and saving this with updates, go back to the terminal (of course in your venv if you make one) and run `dbt debug`. The goal is for all checks to pass. If not, something is wrong.
  - The first time I got a fail, it was because I hadn't installed `dbt-bigquery`, fun right? :clown:
  - The second time I got a fail, the path I was running `dbt debug` from was outside the project folder. i.e. if you want to debug the set up of `jaffle_shop`, make sure you navigate into that directory :clown: :clown:

#### Resources shared by dbt on starter setup files

Try running the following commands:

- dbt run
- dbt test

- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](https://community.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
