Red Team Automation: SQL Injection Testing & Email Scraping

What is this?
During the Technion Cyber Defense & Offense Program's Red Team project, I built this script to demonstrate how SQL injection vulnerabilities actually work in practice. 
It's automated testing for intentionally vulnerable applications (OWASP Juice Shop in this case).

The script does two things:
1.Scrapes email addresses from a web application
2.Tests those emails against a SQL injection vulnerability in the login form

How the vulnerability works
The app has a login form that's vulnerable to SQL injection, instead of properly handling user input, it just concatenates it directly into the SQL query.

If you put in:
Email: admin@example.com'--
Password: anything

The SQL query becomes:
sqlSELECT user FROM accounts WHERE email='admin@example.com'--' AND password=...

Everything after -- is a comment in SQL, so the password check disappears and you're logged in without the actual password.

What you need:
Python 3.8+
Playwright (for browser automation)
Docker (to run the vulnerable test app)

Setup:
Install dependencies:
bashpip install playwright
playwright install
Start the OWASP Juice Shop (the deliberately vulnerable app we're testing against):
bashdocker run -d -p 3000:3000 bkimminich/juice-shop

How to run it:
bashpython exploit.py

The script will:
Open the app in a browser and start clicking through products
Extract email addresses from reviews (they hide them in the product reviews section)
Save all emails to email.csv
Then go through each email and try the SQL injection attack
Log which accounts are actually vulnerable, which have 2FA, which failed

What you get:
After it runs, you'll have:
email.csv - All the emails we found
breached_accounts.csv - Accounts that fell for the SQL injection (the ones without proper protection)
not_accessible.csv - Accounts that have 2FA or other security measures
combined.log - Detailed log of everything that happened

SQL injection is still one of the most common vulnerabilities out there! 
This project shows:
How easy it is to exploit when done wrong
Why proper input validation/parameterized queries are non-negotiable
How attackers automate testing at scale

What I learned building this:
How to use Playwright for proper browser automation (not just basic clicking)
SQL injection mechanics from both sides (the exploit and the defense)
Writing robust automation scripts with proper error handling and logging
The importance of secure coding practices when building web apps

The code is pretty well-commented, if you're learning about security testing or automation, have a look at how it handles dynamic content loading, modal dialogs, and paginated results.

Important disclaimer ⚠️ 
This is for learning purposes only!
I only tested this on OWASP Juice Shop, which is literally designed to be vulnerable for training
Never, ever run this against systems you don't own or have written permission to test
That's not just me being cautious — unauthorized system access is actually illegal
This is a lab project.

Shira Ben David
Technion Cyber Defense & Offense Program (2024-2025)
LinkedIn - https://www.linkedin.com/in/shira-ben-david-620102203/
