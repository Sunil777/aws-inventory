# About #

[![forthebadge](http://forthebadge.com/images/badges/built-with-love.svg)](http://forthebadge.com)
[![forthebadge](http://forthebadge.com/images/badges/powered-by-oxygen.svg)](http://forthebadge.com)
[![forthebadge](http://forthebadge.com/images/badges/fuck-it-ship-it.svg)](http://forthebadge.com)

This script will check all active instances in EC2 in your AWS account

# Prerequisites #
* Python 3.5+
* Boto3 (pip3 install boto3)
* XLWT (pip3 install xlwt)
* SES SMTP Account
* Your AWS Access Key ID and Secret Access Key configured in awscli or IAM Role

# How to send email using smtplib?

Make sure to `import smtplib` first then find these lines...

```python
# Send email
def send_email(subject, msg):
	try:
		client = boto3.client('ses')
		response = client.send_raw_email(
			Source=mail_from,
			Destinations=[
				mail_to
			],
			RawMessage={
				'Data': msg
			}
		)
	except:
		print('ERROR: Message sending failed.')
		sys.exit(1)
```

And replace them with...

```python
# Send email
def send_email(subject, msg):
	smtp_server = 'smtpserver'
	smtp_port = <port>
	smtp_id = 'username'
	smtp_password = 'password'

	try:
		with smtplib.SMTP(smtp_server, smtp_port) as s:
			s.ehlo()
			s.starttls()
			s.ehlo()
			s.login(smtp_id, smtp_password)
			s.sendmail(mail_from, mail_to, msg)
			s.quit()
		print('Report sent to {0}'.format(mail_to))
	except:
		print('ERROR: Message sending failed.')
		sys.exit(1)
```

# Checklist
- [x] EC2
- [x] RDS
- [x] S3
- [ ] VPC
- [ ] CloudFront
- [ ] Route 53