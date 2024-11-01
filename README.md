# CTF Writeups - Software Security Class

This repository contains my write-ups for Capture The Flag (CTF) challenges from my Software Security class. Each challenge explores different security concepts and attack vectors, organized by lab. Below is an overview of the themes and topics covered in each lab.

## Labs and Challenge Themes

### Lab 2 - Introductory Challenges
This lab introduces basic techniques and tools for security testing and CTF challenges:
- **Guess_a_Big_Number.md**: Covers techniques for guessing values in a constrained environment.
- **PwnTools_Sockets.md**: Demonstrates the use of PwnTools for socket programming in exploit development.
- **Python_requests.md** and **Python_requests_Again.md**: Explain how to use Python's requests library for web-based challenges.
- **Secure_by_Design.md**: Discusses security principles for writing secure code from a design perspective.

### Lab 3 - Intermediate Exploits
This lab progresses to more complex exploits with a focus on binary exploitation and race conditions:
- **Another_Jackpot.md**: Describes techniques for exploiting jackpot-style vulnerabilities.
- **I_challenge_you_to_a_race.md**: Explores race condition vulnerabilities in concurrent environments.

### Lab 4 - Web Security and Cookie Manipulation
This lab examines web security challenges, particularly around cookie handling and web application firewalls:
- **Just_my_boring_cookie.md** and **My_favourite_cookies.md**: Explore techniques for manipulating cookies to bypass authentication.
- **Give_me_more_than_a_simple_WAF.md**: Discusses methods for bypassing Web Application Firewalls (WAF).
- **Go_on_and_censor_my_posts.md**: Describes an approach to bypass content filtering.
- **Read_my_lips:No_more_scripts!.md**: Focuses on preventing and exploiting client-side scripting issues.

### Lab 5 - Advanced Exploits
Lab 5 dives into complex vulnerabilities, including financial logic flaws and temporary privilege elevation:
- **Money,_money,_money!.md**: Demonstrates how financial logic flaws can be exploited.
- **Sometimes_we_are_just_temporarily_blind.md** and **Sometimes_we_are_just_temporarily_blind-v2.md**: Cover temporary privilege escalation vulnerabilities.
- **Wow,_it_can't_be_more_juicy_than_this!.md**: Explores advanced techniques in web security and data exfiltration.
- **i_will_take_care_of_this_site.md**: Shows methods for taking over insecure web applications.

### Lab 6 - Buffer Overflows and Function Manipulation
This lab explores buffer overflows, return addresses, and function calls in low-level security:
- **Match_an_Exact_Value.md**: Covers techniques for matching specific values in memory.
- **Calling_functions.md**: Demonstrates calling unauthorized functions via buffer overflow exploits.
- **Return_Address.md**: Explains manipulation of the return address to control program flow.
- **Simple_Overflow.md**: Basic buffer overflow exploitation.
- **Super_secure_lottery.md** and **Super_secure_system.md**: Exploits involving poorly secured systems and applications.

### Lab 7 - Memory Manipulation and Direct Writes
Lab 7 focuses on memory read and write operations for exploiting applications:
- **01_Simple_local_read.md** and **02_short_local_read.md**: Cover basic techniques for reading memory.
- **03_write_to_memory.md**: Shows methods for writing data to arbitrary memory locations.
- **04_write_specific_value.md** and **05_write_specific_byte.md**: Focus on writing precise values and bytes.
- **06_write_big_numbers.md**: Techniques for manipulating larger numerical values in memory.
- **07_Calling_functions_again.md**: Explores repeated function calls for exploit chaining.

---

Each write-up provides step-by-step solutions and explanations of the vulnerabilities addressed in the challenges.
