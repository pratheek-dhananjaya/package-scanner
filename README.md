## Problem Statement:
Every Python code directory comes with a requirements.txt file listing the packages that must be installed before the code can run. Specific versions of those packages can have known security vulnerabilities, and choosing a safe replacement version by hand is slow and easy to get wrong.<br>
## Solution:
Two agents fix vulnerable Python dependencies together: a planner agent proposes upgrades, and a policy reviewer agent checks them against the team's upgrade rules. The user uploads a requirements.txt. Plain code finds the vulnerable packages using the PyPI JSON API. The planner chooses the smallest upgrade that fixes each one. The reviewer checks each proposed version against a plain-English policy, using facts from the same API, and either approves or denies it. If a change is denied, the planner gets one retry, using the reviewer's reason to choose a different version. If the second proposal is also denied, the package keeps its original version with the comment "not updated because of policy limitations," followed by the reason. The result is a corrected requirements.txt and a report explaining every change and decision.
## API:
PyPI JSON API - <br>
- https://pypi.org/pypi/<package>/json - Listing every released version<br>
- https://pypi.org/pypi/<package>/<version>/json - Vulnerabilities and policy facts for one version<br>
## Upgrade Policy:
* Never upgrade across a major version.
* Don't use pre-release versions.
* Don't use a version released in the last 7 days.
* Never choose a yanked version.
* The version must support Python 3.11.
* The proposed version must have no known vulnerabilities.
