<img src="https://github.com/cullenwatson/JobSpy/assets/78247585/ae185b7e-e444-4712-8bb9-fa97f53e896b" width="400">

**JobSpy** is a job scraping library with the goal of aggregating all the jobs from popular job boards with one tool.

## Features

- Scrapes job postings from **LinkedIn**, **Indeed**, **Glassdoor**, **Google**, **ZipRecruiter**, **Bayt** & **Naukri** concurrently
- Aggregates the job postings in a dataframe
- Proxies support to bypass blocking

![jobspy](https://github.com/cullenwatson/JobSpy/assets/78247585/ec7ef355-05f6-4fd3-8161-a817e31c5c57)

### How to use it?

#### 1. Install Poetry if you haven't installed it
Poetry is a tool for dependency management and packaging in Python — it helps you:
- Manage dependencies (like pip)
- Create virtual environments (like venv)
- Build and publish your packages (like setup.py)
- Keep all configurations in a clean pyproject.toml file (no more requirements.txt, setup.cfg, MANIFEST.in clutter!)

Install Poetry using curl (Mac/Linux)
```{shell}
curl -sSL https://install.python-poetry.org | python3 -
```

Then add it to your `PATH` enviroment variable
```{shell}
export PATH="/Users/huanganni/.local/bin:$PATH"
source ~/.zshrc   # apply changes(if using Zsh)
source ~/.bashrc  # apply changes(if using Bash)
```

#### 2. Activate virtual enviroment
```{shell}
env activate
```

#### 3. Install all the dependencies from poetry.lock
If you see error message: `pyproject.toml changed significantly since poetry.lock was last generated. Run poetry lock to fix the lock file.`, you can update poetry lock first `poetry lock` before running the command below.
```{shell}
poetry install
```

#### 4. Run main.py to fetch jobs posted since yesterday
```{shell}
poetry run python main.py
```

_Python version >= [3.10](https://www.python.org/downloads/release/python-3100/) required_

#### Example main function

```python
import csv
from jobspy import scrape_jobs

jobs = scrape_jobs(
    site_name=["indeed", "linkedin", "glassdoor", "google", "bayt"],
    search_term="software engineer",
    google_search_term="software engineer jobs near Singapore since yesterday",
    location="Singapore",
    results_wanted=100,
    hours_old=24,
    country_indeed='Singapore',
    
    # linkedin_fetch_description=True # gets more info such as description, direct job url (slower)
    # proxies=["208.195.175.46:65095", "208.195.175.45:65095", "localhost"],
)
print(f"Found {len(jobs)} jobs")
print(jobs.head())
jobs.to_csv("jobs.csv", quoting=csv.QUOTE_NONNUMERIC, escapechar="\\", index=False) # to_excel
```

### Output

| id                     | site       | job_url                                                                 | title                                       | company                             | location               | date_posted | job_type | salary_source | interval | min_amount | max_amount | currency | is_remote | job_level | job_function | listing_type | emails | description                                                                 | company_industry | company_url | company_logo | company_url_direct | company_addresses | company_num_employees | company_revenue | company_description | skills | experience_range | company_rating | company_reviews_count | vacancy_count | work_from_home_type |
|------------------------|------------|-------------------------------------------------------------------------|---------------------------------------------|-------------------------------------|------------------------|-------------|----------|---------------|----------|------------|------------|----------|-----------|-----------|--------------|--------------|--------|-----------------------------------------------------------------------------|------------------|-------------|--------------|--------------------|-------------------|------------------------|-----------------|---------------------|--------|------------------|----------------|-----------------------|---------------|---------------------|
| bayt-593779292887847651 | bayt       | [link](https://www.bayt.com/en/lebanon/jobs/software-developer-beirut-5297605/) | Software Developer - Beirut                 | Adequate Resources                  | Beirut · Lebanon       |             |          |               |          |            |            |          |           |           |              |              |        |                                                                             |                  |             |              |                    |                   |                        |                 |                     |        |                  |                |                       |               |                     |
| bayt-6644104264792287457 | bayt       | [link](https://www.bayt.com/en/india/jobs/microsoft-copilot-agent-developer-5296608/) | Microsoft Copilot Agent Developer           | Empathy Technologies                | Bengaluru · India      |             |          |               |          |            |            |          |           |           |              |              |        |                                                                             |                  |             |              |                    |                   |                        |                 |                     |        |                  |                |                       |               |                     |
| bayt-7611479627097462454 | bayt       | [link](https://www.bayt.com/en/qatar/jobs/application-developer-5291017/) | Application Developer                       | Futureway Line                      | Doha · Qatar           |             |          |               |          |            |            |          |           |           |              |              |        |                                                                             |                  |             |              |                    |                   |                        |                 |                     |        |                  |                |                       |               |                     |
| bayt-2430312494324618935 | bayt       | [link](https://www.bayt.com/en/uae/jobs/fsd-developer-5289297/)        | FSD Developer                               | Synechron                           | Dubai · UAE            |             |          |               |          |            |            |          |           |           |              |              |        |                                                                             |                  |             |              |                    |                   |                        |                 |                     |        |                  |                |                       |               |                     |
| ... (truncated for brevity) | ...        | ...                                                                     | ...                                         | ...                                 | ...                    | ...         | ...      | ...           | ...      | ...        | ...        | ...      | ...       | ...       | ...          | ...          | ...    | ...                                                                         | ...              | ...         | ...          | ...                | ...               | ...                    | ...             | ...                 | ...    | ...              | ...            | ...                   | ...           | ...                 |
| gd-1009707852379        | glassdoor  | [link](https://www.glassdoor.sg/job-listing/j?jl=1009707852379)        | Software Engineer (Front End)               | TEKsystems (Allegis Group Singapore Pte Ltd) | Singapore              | 2025-04-13  |          |               |          |            |            |          | No        |           |              | sponsored    |        | Collaborate with product team to implement user-friendly interfaces.        |                  |             |              |                    |                   |                        |                 |                     |        |                  |                |                       |               |                     |

### Parameters for `scrape_jobs()`

```plaintext
Optional
├── site_name (list|str): 
|    linkedin, zip_recruiter, indeed, glassdoor, google, bayt
|    (default is all)
│
├── search_term (str)
|
├── google_search_term (str)
|     search term for google jobs. This is the only param for filtering google jobs.
│
├── location (str)
│
├── distance (int): 
|    in miles, default 50
│
├── job_type (str): 
|    fulltime, parttime, internship, contract
│
├── proxies (list): 
|    in format ['user:pass@host:port', 'localhost']
|    each job board scraper will round robin through the proxies
|
├── is_remote (bool)
│
├── results_wanted (int): 
|    number of job results to retrieve for each site specified in 'site_name'
│
├── easy_apply (bool): 
|    filters for jobs that are hosted on the job board site (LinkedIn easy apply filter no longer works)
│
├── description_format (str): 
|    markdown, html (Format type of the job descriptions. Default is markdown.)
│
├── offset (int): 
|    starts the search from an offset (e.g. 25 will start the search from the 25th result)
│
├── hours_old (int): 
|    filters jobs by the number of hours since the job was posted 
|    (ZipRecruiter and Glassdoor round up to next day.)
│
├── verbose (int) {0, 1, 2}: 
|    Controls the verbosity of the runtime printouts 
|    (0 prints only errors, 1 is errors+warnings, 2 is all logs. Default is 2.)

├── linkedin_fetch_description (bool): 
|    fetches full description and direct job url for LinkedIn (Increases requests by O(n))
│
├── linkedin_company_ids (list[int]): 
|    searches for linkedin jobs with specific company ids
|
├── country_indeed (str): 
|    filters the country on Indeed & Glassdoor (see below for correct spelling)
|
├── enforce_annual_salary (bool): 
|    converts wages to annual salary
|
├── ca_cert (str)
|    path to CA Certificate file for proxies
```

```
├── Indeed limitations:
|    Only one from this list can be used in a search:
|    - hours_old
|    - job_type & is_remote
|    - easy_apply
│
└── LinkedIn limitations:
|    Only one from this list can be used in a search:
|    - hours_old
|    - easy_apply
```

## Supported Countries for Job Searching

### **LinkedIn**

LinkedIn searches globally & uses only the `location` parameter. 

### **ZipRecruiter**

ZipRecruiter searches for jobs in **US/Canada** & uses only the `location` parameter.

### **Indeed / Glassdoor**

Indeed & Glassdoor supports most countries, but the `country_indeed` parameter is required. Additionally, use the `location`
parameter to narrow down the location, e.g. city & state if necessary. 

You can specify the following countries when searching on Indeed (use the exact name, * indicates support for Glassdoor):

|                      |              |            |                |
|----------------------|--------------|------------|----------------|
| Argentina            | Australia*   | Austria*   | Bahrain        |
| Belgium*             | Brazil*      | Canada*    | Chile          |
| China                | Colombia     | Costa Rica | Czech Republic |
| Denmark              | Ecuador      | Egypt      | Finland        |
| France*              | Germany*     | Greece     | Hong Kong*     |
| Hungary              | India*       | Indonesia  | Ireland*       |
| Israel               | Italy*       | Japan      | Kuwait         |
| Luxembourg           | Malaysia     | Mexico*    | Morocco        |
| Netherlands*         | New Zealand* | Nigeria    | Norway         |
| Oman                 | Pakistan     | Panama     | Peru           |
| Philippines          | Poland       | Portugal   | Qatar          |
| Romania              | Saudi Arabia | Singapore* | South Africa   |
| South Korea          | Spain*       | Sweden     | Switzerland*   |
| Taiwan               | Thailand     | Turkey     | Ukraine        |
| United Arab Emirates | UK*          | USA*       | Uruguay        |
| Venezuela            | Vietnam*     |            |                |

### **Bayt**

Bayt only uses the search_term parameter currently and searches internationally



## Notes
* Indeed is the best scraper currently with no rate limiting.  
* All the job board endpoints are capped at around 1000 jobs on a given search.  
* LinkedIn is the most restrictive and usually rate limits around the 10th page with one ip. Proxies are a must basically.

## Frequently Asked Questions

---
**Q: Why is Indeed giving unrelated roles?**  
**A:** Indeed searches the description too.

- use - to remove words
- "" for exact match

Example of a good Indeed query

```py
search_term='"engineering intern" software summer (java OR python OR c++) 2025 -tax -marketing'
```

This searches the description/title and must include software, summer, 2025, one of the languages, engineering intern exactly, no tax, no marketing.

---

**Q: No results when using "google"?**  
**A:** You have to use super specific syntax. Search for google jobs on your browser and then whatever pops up in the google jobs search box after applying some filters is what you need to copy & paste into the google_search_term. 

---

**Q: Received a response code 429?**  
**A:** This indicates that you have been blocked by the job board site for sending too many requests. All of the job board sites are aggressive with blocking. We recommend:

- Wait some time between scrapes (site-dependent).
- Try using the proxies param to change your IP address.

---

### JobPost Schema

```plaintext
JobPost
├── title
├── company
├── company_url
├── job_url
├── location
│   ├── country
│   ├── city
│   ├── state
├── is_remote
├── description
├── job_type: fulltime, parttime, internship, contract
├── job_function
│   ├── interval: yearly, monthly, weekly, daily, hourly
│   ├── min_amount
│   ├── max_amount
│   ├── currency
│   └── salary_source: direct_data, description (parsed from posting)
├── date_posted
└── emails

Linkedin specific
└── job_level

Linkedin & Indeed specific
└── company_industry

Indeed specific
├── company_country
├── company_addresses
├── company_employees_label
├── company_revenue_label
├── company_description
└── company_logo

Naukri specific
├── skills
├── experience_range
├── company_rating
├── company_reviews_count
├── vacancy_count
└── work_from_home_type
```
