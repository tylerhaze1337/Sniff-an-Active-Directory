To ensure that GitHub’s markdown parser reads the README correctly, I’ve restructured and formatted your text. Here’s the modified version:


# Full Tutorial: Using Snaffler

## Introduction

Snaffler is an open-source tool designed to help pentesters and red teamers search for sensitive files on a Windows/Active Directory network. It explores accessible SMB shares and identifies files containing critical information such as passwords, SSH keys, configuration files, and more.

## Installation and Prerequisites

### 1. Download Snaffler

Snaffler is available on GitHub: [https://github.com/SnaffCon/Snaffler](https://github.com/SnaffCon/Snaffler)

Clone the repository with:

```bash
git clone https://github.com/SnaffCon/Snaffler.git
```

### 2. Running Snaffler

Snaffler is a Windows executable (.exe). To use it, you need a Windows environment with the .NET framework installed. 

Navigate to the folder containing `Snaffler.exe` and execute:

```bash
Snaffler.exe
```

## Main Commands and Options

### 1. Scan Network Shares

The basic command to start a scan is:

```bash
Snaffler.exe -s
```

This will explore accessible network shares and retrieve sensitive files.

### 2. Specify a Domain or List of Machines

To scan a specific domain or list of machines:

```bash
Snaffler.exe -d MONDOMAINE.LOCAL
```

Or with a list of machines:

```bash
Snaffler.exe -n C:\path\to\machine_list.txt -o C:\path\to\results.txt -v debug
```

### 3. Exclude Specific Folders

To avoid irrelevant folders (logs, temp files, etc.):

```bash
Snaffler.exe -s -k C:\path\to\exclusions.txt
```

Or:

```bash
Snaffler.exe -i "C:\Windows\Logs" -i "C:\Program Files"
```

### 4. Save Results

You can export the results in JSON format for further analysis:

```bash
Snaffler.exe -o results.json -t json
```

## Use Cases

### 1. Searching for Plaintext Passwords

Admins sometimes store passwords in plaintext files (e.g., text or Excel). To search for such files, use Snaffler and look for keywords like:

```bash
Snaffler.exe -smb -regex "password|credentials|login|key"
```

### 2. Identifying Sensitive Files in an Active Directory Environment

Snaffler can be used to scan an Active Directory domain and search for files containing secrets:

```bash
Snaffler.exe -domain MONDOMAINE.LOCAL -regex "secret|admin|backup"
```

## Best Practices and Tips

- **Run scans with an appropriately privileged user:** You need an account with access rights to the network shares.
- **Analyze results with a Python script:** Convert the JSON into CSV for easier filtering of found files.

## List of Snaffler Commands and Their Uses

### 🔍 Main Commands

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-n`, `--comptarget` | Scan a list of computers or a single machine     | `-n C:\path\to\list_of_computers.txt` or `-n PC1,PC2,PC3` |
| `-i`, `--dirtarget`  | Scan a specific folder (without share searching) | `-i C:\Users\Public\Documents`                 |
| `-d`, `--domain`     | Specify an Active Directory domain               | `-d mondomaine.local`                         |
| `-c`, `--domaincontroller` | Specify a domain controller to query        | `-c DC01.mondomaine.local`                    |
| `-a`, `--sharesonly` | Stop after finding shares (without file analysis) | `-a`                                          |
| `-f`, `--dfs`        | Limit the search to DFS shares (OPSEC discretion) | `-f`                                          |

### 📝 Output Options

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-o`, `--outfile`    | Specify a file to store the results              | `-o C:\path\to\results.txt`                   |
| `-s`, `--stdout`     | Display the results live in the console          | `-s`                                          |
| `-t`, `--logtype`    | Log type: `plain` (text) or `json`               | `-t json`                                     |
| `-y`, `--tsv`        | Export the results in TSV (tab-separated values) | `-y`                                          |

### 🔧 Configuration Options

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-z`, `--config`     | Load a `.toml` configuration file               | `-z C:\config.toml`                            |
| `-p`, `--rulespath`  | Load a set of custom rules                      | `-p C:\rules\`                                 |
| `-k`, `--exclusions` | File containing a list of computers to exclude   | `-k C:\exclusions.txt`                        |

### 🚀 Performance & Optimization

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-x`, `--maxthreads` | Number of threads to use (minimum 4)             | `-x 8`                                         |
| `-r`, `--maxgrepsize`| Max file size (in bytes) to analyze             | `-r 500000` (500 KB)                          |
| `-j`, `--grepcontext`| Number of bytes of context around found strings | `-j 200`                                      |
| `-l`, `--snafflesize`| Max size of files copied (in bytes)             | `-l 10485760` (10 MB)                         |

### 🔍 Advanced Search Options

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-u`, `--domainusers`| Retrieve interesting Active Directory users      | `-u`                                          |
| `-b`, `--interest`   | File interest level (0-3)                        | `-b 2`                                         |
| `-m`, `--snaffle`    | Copy found files to a folder                     | `-m C:\path\to\loot`                          |

### 📢 Verbosity & Debugging

| Command             | Description                                      | Example                                        |
|---------------------|--------------------------------------------------|------------------------------------------------|
| `-v`, `--verbosity`  | Log level: Trace, Debug, Info, Data             | `-v debug`                                    |
| `-e`, `--timeout`    | Time between updates (in minutes)               | `-e 10`                                        |
| `-h`, `--help`       | Show help information                           | `-h`                                          |

## Interpreting Results for One Machine

When analyzing the Snaffler scan output, here’s what each section means:

### 📌 Step 1: Initialization

```
[xxxx] 2025-02-10 10:02:44Z [Info] Parsing args...
[xxxxxx] 2025-02-10 10:02:44Z [Info] Parsed args successfully.
```

✅ Snaffler successfully read the command-line arguments. If there's an error here, it means a parameter was incorrectly written.

### 📌 Step 2: Searching for Accessible Shares

```
[xxxxx] 2025-02-10 10:02:44Z [Info] Starting to look for readable shares...
[xxxxx] 2025-02-10 10:02:44Z [Info] Created all sharefinder tasks.
```

✅ Snaffler is starting to search for accessible shares. If it gets stuck here, there might be a domain or machine connection issue.

### 📌 Step 3: Progress Update

```
[xxxx] 2025-02-10 10:07:44Z [Info] Status Update:
ShareFinder Tasks Completed: 0
ShareFinder Tasks Remaining: 0
...
```

🔍 **Analysis**:  
- **ShareFinder Tasks**: Searching for accessible shares.
- **TreeWalker Tasks**: Exploring files and folders in found shares.
- **FileScanner Tasks**: Analyzing files for sensitive content.

🚨 **Possible Issue**: If all tasks are at 0, no shares were found or scanned.  
**Solutions**:  
- Check if the user has the necessary permissions.
- Verify that accessible shares exist.
- Increase execution time with `-e 10` to allow longer scans.

### 📌 Step 4: Rebalancing Workload

```
ShareScanner queue finished, rebalancing workload.
```

🔍 **Analysis**:  
Snaffler adjusts the number of threads dynamically for better performance. If there aren't enough files to scan, the queue will be rebalanced.

### 📌 Step 5: Scan Completion

```
[xxxxxxxx] 2025-02-10 10:07:44Z [Info] Finished at 10/02/2025 11:07:44
```

✅ The scan has completed after 5 minutes.  
🚨 No interesting files were found.

## Using Snaffler-Parser

**Snaffler Parser** is a tool for extracting and analyzing results generated by Snaffler.

### 1. Prerequisites

Ensure you have installed:

- **Snaffler**: Run Snaffler with the `-y` argument for JSON output.
- **Snaffler Parser**: Script for analyzing results.

### 2. Running Snaffler with the `-y` Argument

Ensure you run Snaffler with the `-y` argument to get JSON output:

```bash
Snaffler.exe -y -n machine.txt -i "C:" -b 0 -r 10000000 -j 500 -o results.txt -s
```

### 3. Using Snaffler Parser

To parse the results with Snaffler Parser:

```bash
.\snafflerparser.ps1 -in [path_to_snaffler_results.txt] -y
```

This generates detailed reports in HTML, CSV, TXT, etc., listing the detected sensitive files with priority and relevance.

### 4. Snaffler Output File Parser

In large environments, manual analysis can be tedious. Snaffler Parser automates this process by parsing the TSV files generated by Snaffler.

#### Key Features:
- Generate reports in TXT, CSV, HTML, JSON, PS Gridview
- Filter by severity and file extensions
- Highlight detected keywords
- Export accessible

 files for further analysis

### Example command for parsing:

```powershell
.\SnafflerParser.ps1 -in "C:\Results\results.tsv"
```

This command produces a list of files matching the configured keywords, which can then be processed further.

## Conclusion

Snaffler is a powerful tool for identifying sensitive files across a network. With the ability to search specific domains, computers, and file types, it provides a comprehensive solution for red teamers and penetration testers. By using advanced features like custom configurations and parallel processing, you can tailor the scan to your needs and efficiently analyze the results. Happy hunting!
```

This version maintains clarity, adds proper markdown formatting, and organizes the commands and examples. Let me know if you’d like any more changes!
