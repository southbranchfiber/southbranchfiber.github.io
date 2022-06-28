This is our website, hosted on Github Pages. Github Pages is easy to
use, performant, and free.  It also supports the Jekyll static site
builder. All in all it's a good product. But it's not so well
documented, hence this README.

To start a static website for a small organization:

0. We assume you have a personal Github account, connected to your
local git workspace in whatever way (https/ssh/etc.) you prefer. If
you don't have one, create one now. If you don't use it for this
you'll use it for something else.

1. Create a Github account for the organization. This can be an
ordinary developer account. Let's assume for this README that the new
account is named `southbranchfiber`.

2. Signed in to Github as that account, create an empty repository
`southbranchfiber.github.io`. Then in Settings / Collaborators, add your
personal account as a collaborator for the new empty repository.

* NOTE that the repository must be the name of your account followed by `.github.io`

3. Create a branch named `main` in the new repository, and use your
local account to create a placeholder `index.html` or `index.md` file
in the root of that branch. Push your local repo to Github and check
that the placeholder file is in branch `main` of
`southbranchfiber.github.io`.

4. Still signed into Github as the organization's account, go to
Settings / Pages and turn on publishing. Publish from branch `main`
starting at the root folder. In the theme chooser, select "Minimal"
for now. Don't set a custom domain yet.

5. Verify that the placeholder page appears at http://southbranchfiber.github.io/ .

**Congratulations!** At this point, you have a placeholder page up on
Github Pages -- without a custom domain, sure, but first things first.

Next we need to create a staging site. In this example, we'll create
the staging site in your personal account. If you already use Github
Pages for your personal account then you're too advanced for this
README :) so we'll assume that you don't.

| Can you get by without a staging site? For sites that no one depends
| on, sure. You can test your changes locally before you commit them,
| and *most of the time* the live site will behave the same way. But
| if you're using Windows the "test changes locally" thing doesn't
| really work, and even on Unix it's not completely reliable. If it
| really should work the first time, there's no substitute for a
| staging site.

| Siteleaf provides a Jekyll-aware staging service that integrates
| well with Github Pages. You have to pay $7/mo for the staging
| facility though.

6. Sign out of the organization's Github account and sign into your
personal Github account. Let's assume for this README that the
personal account is named `dulitz`.

7. Visit the repository `southbranch/southbranch.github.io`, and fork
it. The owner of the fork should be `dulitz` and the name of the
forked repository should be `dulitz.github.io`.

8. In the repository `dulitz/dulitz.github.io`, go to Settings / Pages
and turn on publishing. Publish from branch `main` starting at the
root folder. In the theme chooser, select "Minimal" for now. Don't
set a custom domain.

9. Verify that the placeholder page appears at http://dulitz.github.io/ .

**Congratulations!** At this point you have a placeholder live page
and a working staging site. If you aren't going to use Jekyll (because
you want to use some other site builder) you are basically done.

But assuming you want to use Jekyll like we do, the next task is to
set up your local Jekyll build environment. We'll assume you're not
using Windows. (I'll happily merge a PR describing a similar process
for Windows.)

10. Start by installing Ruby and the Ruby development
framework. Usually those packages are named `ruby` and either
`ruby-dev` or `ruby-devel` depending on your distro and package
manager.

11. Check if bundler is installed (`which bundle`). If it isn't,
install it using `sudo gem install bundler`

12. Pull the staging site into your local workspace.

13. In the root of the staging site, initialize the bundler with
`bundle init`

14. Run `bundle config set --local path vendor/bundle` so that all
bundle code is stored locally instead of in a central location on your
system.

We haven't run a test yet, so no congratulations at this point, but
we'll give you an A for effort. The next step is to get the bundler to
install all the gems necessary to build the site locally. The bundler
looks to `Gemfile` to determine what is needed, so we need to create a
valid Gemfile.

15. If we've kept our code up to date, you can probably start with a
[our Gemfile here](/Gemfile).

15a. If that doesn't work (maybe this repo has gone stale), the most
important lines are probably:
```
gem "minima"
gem "jekyll-remote-theme"
gem "github-pages", group: :jekyll_plugins
```

16. Once your Gemfile is good, run
```
bundle install
```
Ideally it will complete with no errors and show "Bundle updated!"
Otherwise fix the error by changing Gemfile.

17. You should now be able to run `bundle exec jekyll serve` and then
view your site at [http://localhost:4000/](http://localhost:4000/) .

18. Add `Gemfile` to your repo. Now is also a good time to add a
.gitignore file such as [ours](/.gitignore).

**Congratulations!** You're now able to build and serve your site with
Jekyll on your local machine. But Jekyll isn't really doing anything
at this point, since no files have front matter and Jekyll has no
configuration file.

19. Now create your `_config.yml` to tell Jekyll how to build your
site. You can use [ours](/_config.yml) as a template if you want to.

20. If you don't add the plugin `github-remote-theme`, Github will
build your site with the theme you've selected in Github. If you do
add that plugin, the `remote_theme` config variale should point to a
Github-hosted theme template.

21. In addition to the values in `_config.yml`, you should update the
logos in /assets, and probably `/assets/default-social-image.png`. You
should also probably update the default CSS in `/assets/styles.scss`,
if you want to change fonts or something.

22. /404.html is the custom 404 page, and you should change it to be
whatever you want. In order to work in Github, it _must_ have front
matter that explicitly defines its `permalink` as `/404.html`. (That
confused me to no end.)

23. Test the site by running `bundle exec jekyll serve` and looking at
[http://localhost:4000/](http://localhost:4000/).

24. If you like it, commit to your repo and push to the staging
site. Within a minute or so, you should be able to see it at -- in the
example of this README -- `http://dulitz.github.io/`.

25. If you like that, create a PR from the staging site to the live
site's repo `southbranchfiber/southbranchfiber.github.io`. Within a
minute or so after integrating that PR, you should be able to see the
new site at -- in the example of this README --
`http://southbranchfiber.github.io/`.

**Congratulations!** You now have a live website that you like. Good
work.

The next step is to enable a custom domain for the live
site. Since this README is written for small organizations with a
static website, we will enable the website to serve from both the
"apex domain" (in this case `southbranch.net`) and the www subdomain
(`www.southbranch.net`).

26. Signed in to Github as `southbranchfiber`, go to Settings / Pages
and set your apex domain (here, `southbranch.net`) as your custom
domain; then press `Save`. The DNS check will fail but ignore that for
now.

27. Go to your DNS provider's DNS settings (for example,
`https://domains.google.com/registrar/southbranch.net/dns`), set www
as a CNAME for `southbranchfiber.github.io`, and set the A and AAAA
records to the IP addresses that Github documents under [managing a
Github Pages custom
domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain).

27a. If your domain provider supports the `ANAME` pseudo-record, add
an ANAME for your apex domain pointing to
`southbranchfiber.github.io`.

28. Now you should be able to go back to Github Settings / Pages and
it should show that the DNS check succeeds.

29. Verify that your site is available via HTTP at both at your apex
domain and your www subdomain. If one of those doesn't work, try
turning off the custom domain and turning it back on again (still set
to your apex domain).

30. Over the next few minutes, Github should automatically provision
your site with a TLS certificate from Let's Encrypt.

31. Once the TLS cert has been issued, and your site is available over
HTTPS at both domains, and the DNS check succeeds, you should be able
to go to Settings / Pages and turn on "force HTTPS."

**Congratulations!** You have a website. You like it. Hopefully it
looks good. It's performant. It's maintainable. It's free.

One final tip: Remember that, since Github Pages hosting is free, your
repository can't be too large. Keep those PNGs small!

# Futher reading

[Github: testing your Pages site locally](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
[Github: problems people have had with local builds of Github Pages](https://github.com/github/docs/issues/2177)
