# Caller ID Log Viewer

A simple web-based Caller ID log viewer that reads `cidcall.log` from NCID (Network Caller ID) and displays it in a user-friendly table format. The current implementation uses JavaScript to fetch and parse the log file, formatting the date and time for better readability, providing a lookup for unknown numbers, and offering features like log downloads and adjustable call history display.

## Features
- **Displays caller information** (Date, Time, Number, Name)
- **Formats dates** into UK format (DD/MM/YYYY) and recognizes 'Today' and 'Yesterday'
- **Live updates every 5 seconds** (only updates if new entries appear)
- **Number lookup** for unknown callers via [who-called.co.uk](https://who-called.co.uk)
- **Option to change history length** (Last 10, 20, 50, or All calls)
- **Download log file** as `cidcall.log`
- **Audio notification** when a new call appears
- **Mobile responsive design**
- **Cache-busting technique** to ensure the latest log is always fetched

---

## Installation & Setup

### Prerequisites
Ensure you have NCID (`ncid`) installed and running - https://ncid.sourceforge.io/index.html. 

The log file is typically stored in:
```
/var/log/ncid/cidcall.log
```

### Step 1: Clone the Repository
```sh
git clone https://github.com/allanstevens/PhoneLog.git
cd caller-id-log-viewer
```
Or just download the files from here.

### Step 2: Set Up Symbolic Link
Since JavaScript in a browser cannot access system log files directly, you need to create a symbolic link in your web server's `www` directory:

```sh
ln -s /var/log/ncid/cidcall.log /var/www/html/cidcall.log
```

Make sure the web server has permission to read the log file:
```sh
chmod 644 /var/log/ncid/cidcall.log
```

### Step 3: Deploy to Web Server
Copy the `index.html` file to your web server directory:
```sh
cp index.html /var/www/html/
```
Restart the web server if needed:
```sh
systemctl restart apache2  # For Apache
systemctl restart nginx    # For Nginx
```

---

## Usage
- Open the web page in a browser: `http://yourserver/PhoneLog`
- Adjust the call log display using the dropdown menu
- Click **Download Log** to save a copy of the log file
- Unknown numbers can be clicked to search [who-called.co.uk](https://who-called.co.uk)
- A beep sound will play when new calls are detected

---

## Future Improvements & Later Versions
The current JavaScript-based version is simple but can be improved significantly. Here are some potential enhancements:

### **PHP-based Solution**
Instead of using JavaScript to fetch the log file, a **PHP script** can read the file and serve it dynamically. This would:
- Remove the need for a symbolic link (`ln -s`)
- Allow real-time updates without needing a full reload accessed via seeking at the end or using tail, which would be more efficient then downlading the whole file via javascript
- Improve security by restricting direct access to the log file

### **Authentication & Access Control**
- Restrict log access using user authentication (e.g., login system)
- Log and monitor access to caller history

### **Alternative Solutions**
While this setup works well for simple cases, other solutions might be better.  Check out the [ncid.sourceforge.io](https://ncid.sourceforge.io) website for other solutions.

---

## Troubleshooting
### **Log File Not Updating**
1. Ensure NCID is running and logging calls:
   ```sh
   tail -f /var/log/ncid/cidcall.log
   ```
2. Verify web server permissions:
   ```sh
   ls -l /var/log/ncid/cidcall.log
   ```
3. Check JavaScript errors in the browser console (`F12` > Console)

### **Log File Not Found in Web Page**
- Make sure the symbolic link exists and is accessible by the web server:
  ```sh
  ls -l /var/www/html/cidcall.log
  ```
- Try using `chmod 644 /var/log/ncid/cidcall.log` to fix permissions

### **New Calls Not Detected**
- Ensure the browser is not caching old logs (`Shift + Refresh`)
- Try opening the log file manually and verifying new calls are recorded

---

## License
This project is licensed under the MIT License.

---

## Contributing
Pull requests and feature suggestions are welcome! If you'd like to contribute, fork the repo and submit a PR with improvements.

Happy coding! 🚀
