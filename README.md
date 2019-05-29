# Hosting a Static Site on Github Pages with Hugo

Just read [the step-by-step-instructions](https://gohugo.io/hosting-and-deployment/hosting-on-github/#step-by-step-instructions)
on Hugo's website for a quick start guide.

---

## Setting up the Repo

**Clone this repo (with its submodules)**

```
git clone --recursive <git URL>
```

**Install Hugo**

```
brew install hugo
```

**Run the Hugo Server**

This allows you to view your post before committing/publishing. The server
should be accessible at `http://localhost:1313/`

```
hugo server
```

**Write a new Post**

Write a new Markdown file in the `content` directory or one of its
sub-directories. As soon as your file is saved, the local site contents will be
updated.

**Publish**

Convert and deploy the finished post to the static site.

```
./deploy.sh
```

Commit and push the changes to this repo.
