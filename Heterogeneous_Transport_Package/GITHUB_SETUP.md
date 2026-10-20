# Publish the Three Prepared Repositories

The code and findings are prepared, tested and committed locally. Remote GitHub
publication is the remaining step. These are proposed repository names, not
claims that the URLs already exist:

- `transport-findings`
- `transport-generator`
- `transport-algorithms`

## Option 1: publish source folders

Create three empty repositories through https://github.com/new in the account
or course organization you intend to use. Choose visibility according to the
team's course policy. Do not initialize remote README files if you want a simple
first push. Then run these commands separately inside each matching source
folder, replacing REMOTE_URL with the actual newly created repository URL:

```powershell
git init -b main
git add .
git commit -m "Initial research and prototype"
git remote add origin REMOTE_URL
git push -u origin main
```

Git must be installed and authenticated, and your Git author identity must be
configured. The extracted source folders exclude local Git internals; use the
included bundles below if you want to preserve the prepared commit history.

## Option 2: preserve the prepared history

From a separate empty working directory, clone each corresponding bundle:

```powershell
git clone PATH_TO_BUNDLE/transport-findings.bundle transport-findings
```

Replace PATH_TO_BUNDLE with the location of this package's bundles folder. Repeat
for the generator and algorithms. The initial `origin` points to the local bundle;
replace it with the actual GitHub repository URL before pushing:

```powershell
git remote set-url origin REMOTE_URL
git push -u origin main
```

Share the resulting three actual repository links with the supervisor. Data and
code licenses are kept separate. All included Amazon catalog data are from the
small sample whose metadata reports CC0, not a downloaded commercial full dataset.
