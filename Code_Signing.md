# Instructions for code signing using SSH keys and github

These instructions are tested on Ubuntu, but I hope similar procedures work on other platforms.

1. First, [generate a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).

You should give it a different name from the SSH key you use for authentication, for example when prompted for a file in which to save it you might choose
```
/home/YOU/.ssh/id_ed25519_signing
```

It's acceptable to skip this step and use the same key for signing and authentication, but it's discouraged because you probably want to refresh your authentication key more often than your signing key, and if an authentication key is leaked you don't want all your earlier signed
commits to look unsafe.

2. Tell git that you want to use the new key for signing
```
git config --global user.signingkey ~/.ssh/id_ed25519_signing.pub
```

3. Upload the public key `id_ed25519_signing.pub` to your github account using [these instructions.](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) Specify that it's a signing key (not an authentication key).

4. Now you can [sign commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits) with
```
git commit -S -m "YOUR_COMMIT_MESSAGE"
git push

```

5. If it doesn't work, you might need to update some parts of your git configuration. Try
```
git config --global gpg.format ssh
```
and look for other instructions at [the github docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key). If you get an error that ssh is a bad config for gpg.format, you need to [make sure you have the latest version of git](https://git-scm.com/).

6. (Optional) If you don’t want to type the -S flag every time you commit, tell Git to sign your commits automatically:
```
git config --global commit.gpgsign true
```
