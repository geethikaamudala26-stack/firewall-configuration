# firewall-configuration
🔥 Firewall Configuration and Testing Using UFW – Kali Linux

📌 Objective

To configure and test a firewall in Kali Linux using UFW (Uncomplicated Firewall) by:

- Checking existing firewall rules
- Blocking inbound traffic on TCP port 23 (Telnet)
- Testing the blocked port
- Allowing SSH traffic on TCP port 22
- Removing the test rule
- Understanding how a firewall filters network traffic

---

🛠️ Tools Used

- Kali Linux
- UFW (Uncomplicated Firewall)
- Terminal
- Netcat ("nc")
- Nmap

---

1. Check UFW Installation and Status

Open the Kali Linux Terminal and run:

sudo ufw status

If UFW is not installed:

sudo apt update
sudo apt install ufw

Check the detailed firewall status:

sudo ufw status verbose

---

2. Enable the Firewall

Enable UFW:

sudo ufw enable

Verify:

sudo ufw status

«⚠️ If you are connected to Kali through SSH, allow SSH before enabling UFW to avoid losing access.»

---

3. List Current Firewall Rules

Display the firewall rules with numbers:

sudo ufw status numbered

This allows individual rules to be identified and removed later.

---

4. Block Inbound Traffic on Port 23

Port 23/TCP is commonly associated with Telnet.

Create a firewall rule to deny TCP traffic on port 23:

sudo ufw deny 23/tcp

Verify the rule:

sudo ufw status numbered

Expected result:

23/tcp    DENY    Anywhere

This rule blocks incoming TCP connections to port 23.

---

5. Test Port 23

First check whether a service is listening on port 23:

sudo ss -lntp | grep :23

Test the port locally using Netcat:

nc -vz 127.0.0.1 23

You can also use Nmap:

nmap -p 23 127.0.0.1

If no service is running on port 23, the connection may fail even without the firewall. Therefore, the test demonstrates the port's accessibility rather than proving the firewall alone caused the failure.

---

6. Allow SSH Traffic on Port 22

If SSH is required on the Kali machine, allow TCP port 22:

sudo ufw allow 22/tcp

Alternatively:

sudo ufw allow ssh

Check the rules:

sudo ufw status numbered

Expected configuration may look like:

22/tcp    ALLOW    Anywhere
23/tcp    DENY     Anywhere

«⚠️ Do not remove the SSH rule if you are currently accessing Kali through SSH.»

---

7. Verify Firewall Configuration

Run:

sudo ufw status verbose

This displays the current firewall status and configured rules.

---

8. Remove the Test Block Rule

After completing the test, remove the temporary port 23 rule:

sudo ufw delete deny 23/tcp

Verify that it has been removed:

sudo ufw status numbered

---

9. Optional – Remove the SSH Rule

If the SSH rule was added only for this practical and is no longer required:

sudo ufw delete allow 22/tcp

Do not remove it if SSH access is still required.

---

10. Final Firewall Check

Run:

sudo ufw status verbose

This confirms the final firewall configuration after removing the temporary test rule.


🔍 How a Firewall Filters Traffic

A firewall controls network traffic by comparing incoming or outgoing packets against configured rules.

The basic process is:

Network Traffic
       ↓
   Firewall
       ↓
Check Rules
       ↓
 ┌─────┴─────┐
 ↓           ↓
ALLOW       DENY
 ↓           ↓
Traffic     Blocked
Allowed     Traffic

In this practical:

- TCP port 23 was blocked to demonstrate denial of unwanted inbound traffic.
- TCP port 22 was allowed for SSH when required.
- The temporary port 23 rule was removed after testing.

---

✅ Conclusion

The UFW firewall was successfully configured and tested on Kali Linux. A rule was created to block inbound TCP traffic on port 23, the firewall configuration was verified, SSH traffic on port 22 was allowed when required, and the temporary test rule was removed to restore the configuration.

This practical demonstrates how host-based firewalls can control network access by applying rules based on ports, protocols, traffic direction, and network addresses.

---

🧠 Key Commands

# Check firewall
sudo ufw status verbose

# Enable firewall
sudo ufw enable

# View rules
sudo ufw status numbered

# Block Telnet
sudo ufw deny 23/tcp

# Allow SSH
sudo ufw allow 22/tcp

# Test port
nc -vz 127.0.0.1 23

# Scan port
nmap -p 23 127.0.0.1

# Remove test rule
sudo ufw delete deny 23/tcp

# Final check
sudo ufw status numbered
