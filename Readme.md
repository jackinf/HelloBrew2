# Example app for using a Homebrew

This app is an example app that can be installed using Homebrew.

## Prerequisites

### Line endings

In IDE, make sure that line endings are \n (Linux/MacOS), not \r\n (Windows). Otherwise, the script will fail.

### Creation of a Release

First, create a tar file.

Windows:
```powerhsell
@echo off
mkdir C:\Users\<user>\tars
git archive --format=tar.gz --output C:\Users\<user>\tars\hello-brew.tar.gz HEAD
```

Then, retrieve its SHA256 hash.

Windows:
```powershell
Get-FileHash -Path C:\Users\<user>\tars\hello-brew.tar.gz  -Algorithm SHA256
```

Then in GitHub, create a release and upload the tar file. In the release description, put the SHA256 hash.

### Formula creation

Create a GitHub repo called <username>/homebrew-tap. For example, if your GitHub username is `johndoe`, create a repo called `johndoe/homebrew-tap`.
Inside, create a folder called `Formula` and put your formula there. The formula should be named `hello-brew.rb`.

Formula example:

```ruby
class HelloBrew < Formula
  include Language::Python::Virtualenv

  desc "Example Python console app to play with Homebrew"
  homepage "https://github.com/<user>/HellowBrew"
  url "https://github.com/<user>/homebrew-tap/releases/download/<version>/<tar-name>.tar.gz"
  sha256 "<sha>"
  license "MIT"

  # depends_on "python@3.11"

  def install
    bin.install "hello-brew.py" => "hello-brew"
    chmod 0755, bin/"hello-brew"
  end

  test do
    system "#{bin}/hello-brew"
  end
end
```