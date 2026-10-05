# Release Tools

Tools used to create new release for our packages. Can be used for any Github Package to create release tags and changelogs.

## Tag a new Release

To tag a new version on a specific branch the following command can be used:

```bash
bin/tag-release sulu/sulu 2.6
```

The command clones the branch, proposes the new tag and asks before it pushes the tag to the repository.

It stops with an error when the head of the branch is already tagged, when the new tag already exists
or when a git command (clone, tag, push) fails. It warns and only continues on a `y` when

 - the previous tag is not merged into the branch,
 - the `composer.json` requires only a development version of a package,
 - the `UPGRADE-*.x.md` files got notes under a version the previous tag already contains,
   or contain a section of a version which is not released yet,
 - the `composer.json` of the skeleton does not require `sulu/sulu` as `~<new tag>`.

Without a terminal (script or agent) the command refuses to run, because every question would be answered with its
default. Pass `--yes` to accept the defaults, a warning is answered with `n` then.

### First release of a new version

A branch without any tag, for example `3.1`, gets `3.1.0` as proposed tag and uses the latest tag below it
(`3.0.x`) as previous tag for the changelog. Merge the previous version into the branch first (`3.0` into `3.1`),
the command checks that the previous tag is merged:

```bash
bin/tag-release sulu/sulu 3.1
bin/generate-changelog sulu/sulu 3.0.12...3.1.0
```

## Generate Changelog

To generate a changelog for a new created tag the following command can be used:

```bash
bin/generate-changelog sulu/sulu 2.6.12...2.6.13
```

After the changelog is generated and all tests working correctly add the changelog
over the [Github UI](https://github.com/sulu/sulu/tags) to the previous created tag.
To do this go to the tags overview click on the newly pushed tag `...` and click `Add release`
and copy the generated changelog into the title and description field.

For `sulu/sulu` and `sulu/skeleton` we copy the changelog together both contain the whole changelog.
Recommended to create the Release on Github after tested the tags locally.

## Create Skeleton Tag

If a new Version of `sulu/sulu` is released also a new release of the skeleton has to be created.

To achieve this begin on the lowest version in this example 2.6.

```git
# it is recommended do this on a clean new directory:
cd /tmp
git clone git@github.com:sulu/skeleton.git skeleton-2.6
cd skeleton-2.6

# merge the previous version into this branch (2.6 into 3.0 or 3.0 into 3.1), not needed on the lowest version
git pull origin 2.6
# conflicts are expected in composer.json (keep the sulu/sulu constraint of this branch)
# and in public/build/admin (remove the directory, it is rebuilt below)

# update version constraint
vim composer.json # update sulu/sulu version constraint to ~<new version>, e.g. ~2.6.27
composer update

# create admin build (recommended to use a node or npm version which is a currently supported LTS)
cd assets/admin
npm install
npm run build

# push the build
git add -A
git commit -m "Bump Version"
git push origin 2.6 # the branch requires a pull request, the push works for maintainers who can bypass the rule
```

Now also tag the skeleton:

```bash
bin/tag-release sulu/skeleton 2.6
```

Do the same for the upper version (3.0, 3.1, ...).
