
***
###### `Title :- Analysing logs and alerts`
###### `Created on :- 2023-08-24 - 22:42`
###### `Created by:- Prem J`
***

- SIEM tools gets all the security related logs ingested through agents, port forwarding, etc.
- Once the logs are ingested SIEM looks for unwanted behaviour and patterns within the logs with the help of rules set.

#### `Dashboard -->`

- Contains the summary of the analysis, for instance

- Alert Highlights
- System Notification
- Health Alert
- List of Failed Login Attempts
- Events Ingested Count
- Rules triggered
- Top Domains Visited

![[Pasted image 20230824224503.png]]

#### `Correlation Rules -->`

- Used to identify the unwanted behaviour or identifying the threats. For instance, some of the rules are

	- If a User gets 5 failed Login Attempts in 10 seconds - Raise an alert for `Multiple Failed Login Attempts`
	- If login is successful after multiple failed login attempts - Raise an alert for `Successful Login After multiple Login Attempts`
	- A rule is set to alert every time a user plugs in a USB (Useful if USB is restricted as per the company policy)
	- If outbound traffic is > 25 MB - Raise an alert to potential Data exfiltration Attempt (Usually, it depends on the company policy)

#### `How a correlation rule is created -->`

To explain how the rule works, consider the following Eventlog use cases:

**Use-Case 1:**

Adversaries tend to remove the logs during the post-exploitation phase to remove their tracks. A unique Event ID **104** is logged every time a user tries to remove or clear event logs. To create a rule based on this activity, we can set the condition as follows:

**Rule:** If the Log source is WinEventLog **AND** EventID is **104** - Trigger an alert `Event Log Cleared`

**Use-Case 2:** Adversaries use commands like **whoami** after the exploitation/privilege escalation phase. The following Fields will be helpful to include in the rule.

- Log source: Identify the log source capturing the event logs      
- Event ID: which Event ID is associated with Process Execution activity? In this case, event id 4688 will be helpful.      
- NewProcessName: which process name will be helpful to include in the rule?      

**Rule:** If Log Source is WinEventLog **AND** EventCode is **4688,** and NewProcessName contains **whoami,** then Trigger an ALERT `WHOAMI command Execution DETECTED`

In the previous task, the importance of field-value pairs was discussed. Correlation rules keep an eye on the values of certain fields to get triggered. That is the reason why it is important to have normalized logs ingested.

#### `Alert Investigation -->`

- Alert is False Alarm. It may require tuning the rule to avoid similar False positives from occurring again.      
- Alert is True Positive. Perform further investigation.      
- Contact the asset owner to inquire about the activity.
- Suspicious activity is confirmed. Isolate the infected host.
- Block the suspicious IP.