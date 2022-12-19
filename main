from flask import Flask, request, render_template
import smtplib
import csv
import time

app = Flask(__name__)

# Read in the CSV file and SMTP host from the user
csv_file = "emails.csv"  # specify a default CSV file
smtp_host = "smtp.zoho.com"  # specify a default SMTP host

@app.route("/", methods=["GET", "POST"])
def home():
    if request.method == "POST":
        # Read the form data
        csv_file = request.form["csv_file"]
        smtp_host = request.form["smtp_host"]
        smtp_username = request.form["smtp_username"]
        smtp_password = request.form["smtp_password"]

        # Read in the CSV file
        with open(csv_file, "r") as f:
            reader = csv.DictReader(f)
            num_emails = sum(1 for row in reader)  # count the number of rows in the CSV file
            f.seek(0)  # reset the file pointer to the beginning of the file
            reader = csv.DictReader(f)  # re-create the reader object

            # Connect to the SMTP server
            SMTP_PORT = 587  # default port for SMTP with STARTTLS
            server = smtplib.SMTP(smtp_host, SMTP_PORT)
            server.starttls()
            server.login(smtp_username, smtp_password)

            # Send an email to each address in the CSV file
            count = 0  # initialize the counter
            for row in reader:
                recipient = row["Email"]
                subject = row.get("Subject", "")  # use the subject from the CSV file if available, otherwise use an empty string
                message = row["Message"]
                msg = f"Subject: {subject}\n\n{message}"
                server.sendmail(smtp_username, recipient, msg)

                # Increment the counter and display the progress status
                count += 1
                print(f"Sent email {count}/{num_emails} to {recipient}")

                # Delay for 2 seconds before sending the next email
                time.sleep(2)

            # Disconnect from the server
            server.quit()

        return "Emails sent successfully!"
    else:
        return render_template("index.html")

if __name__ == "__main__":
    app.run()
