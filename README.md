# dotnet-framework-auto-version

A GitHub Actions workflow that **automatically bumps the version** of your .NET Framework project (`AssemblyInfo.cs`) on every push to the `main` (or `master`) branch.  
It also commits the change, creates a Git tag, and pushes it back to the repo.

---

## ✨ Features
- 🔍 Finds your `AssemblyInfo.cs` automatically  
- 🔢 Increments the **patch version** (`21.4.55.0 → 21.4.56.0`)  
- 📦 Updates both:
  - `AssemblyVersion` → ends with `.0`
  - `AssemblyFileVersion` → ends with `.1`
- 📝 Commits changes with message `vX.Y.Z.W`
- 🏷️ Creates and pushes a Git tag with the new version
- ⚡ Supports **push to main/master** and **manual runs** (`workflow_dispatch`)

---

## 📂 Example before & after

**Before:**
```csharp
[assembly: AssemblyVersion("21.4.55.0")]
[assembly: AssemblyFileVersion("21.4.55.1")]
```

**After:**
```csharp
[assembly: AssemblyVersion("21.4.56.0")]
[assembly: AssemblyFileVersion("21.4.56.1")]
```

---

## 🛠️ Usage

Add this workflow file to your repo:

**`.github/workflows/bump-version.yml`**

```yaml
name: Auto Bump Repo Version

on:
  push:
    branches:
      - main     # or "master" if your repo uses that
  workflow_dispatch:

jobs:
  bump:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Git
        run: |
          git config user.name "CI Bot"
          git config user.email "ci-bot@example.com"

      - name: Bump version, commit and tag
        shell: pwsh
        run: |
          # (PowerShell script here — see repo for full script)
```

---

## ⚠️ Notes
- ✅ Works with **.NET Framework (AssemblyInfo.cs)** projects  
- ❌ Does **not yet support .NET 5+/6+/7+** (where versioning lives in `.csproj`)  
- For multi-project solutions, it bumps the **first AssemblyInfo.cs** found (can be extended to loop over all)

---

## 🤝 Contributing
Pull requests are welcome!  
If you’d like to add support for other versioning formats (e.g. `.csproj`), feel free to fork and submit improvements.
