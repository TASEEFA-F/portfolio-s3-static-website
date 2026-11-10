# Personal Portfolio: Static Website Hosting on Amazon S3

My cybersecurity portfolio, hosted as a static website on Amazon S3. There is no server to run or patch: one bucket serves the page to anyone with the link.

Built as part of the **AWS re/Start** program with **ICT Academy**.

**Live site:** `http://YOUR-BUCKET-NAME.s3-website-us-east-1.amazonaws.com` (replace with your bucket website endpoint)

## About the site

The site has a security theme. Each section is an "open port" from an Nmap-style scan:

- `whoami` (about me)
- `experience` (internships and hackathons)
- `projects`
- `certifications`
- `skills`
- `contact`

It is a single `index.html` file with plain HTML, CSS and JavaScript (parallax background, a typing terminal and scroll animations), and it works on phones and desktops.

## Architecture

```
Visitor's browser
      |  HTTP
      v
Amazon S3 bucket (static website hosting)
      |
      v
index.html
```

Access is controlled by a bucket policy that allows public **read-only** access (`s3:GetObject`) to the files, and nothing else.

## Files

```
portfolio-s3-static-website/
├── index.html           the portfolio
├── bucket-policy.json   public read-only bucket policy
└── README.md
```

## How to deploy it

1. In the S3 console, create a bucket in US East (N. Virginia).
2. Untick **Block all public access** and confirm the warning.
3. Upload `index.html`.
4. **Properties** > **Static website hosting** > **Enable**, with `index.html` as the index document.
5. **Permissions** > **Bucket policy**: paste `bucket-policy.json`, replacing `YOUR-BUCKET-NAME` with your bucket name. Keep the `/*` at the end.
6. Open the **Bucket website endpoint** link from the Properties tab.

To update the site, upload a new `index.html` to the same bucket. It replaces the old file.

## Common errors

| Error | Cause |
| --- | --- |
| 403 Forbidden | Block public access is still on, or the bucket policy is missing or has no `/*` |
| 404 Not Found | The file is not named exactly `index.html` |

## What I learned

- For a static site, S3 is simpler and cheaper than running a server.
- Public access should be deliberate: the bucket policy makes the files readable without letting anyone change them.
