# Release Tools

Tools used to create new release for our packages. Can be used for any Github Package to create release tags and changelogs.

## Tag a new Release

To tag a new version on a specific branch the following command can be used:

```bash
bin/tag-release sulu/sulu 2.6
```

Answer the question and it will push the new tag to the repository.

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

# merge previous versions into this branch (2.5 into 2.6 or 2.6 into 3.0)
git pull origin 2.5

# update version constraint
vim composer.json # update sulu/sulu version constraint to new version
composer update

# create admin build (recommended to use a node or npm version which is a currently supported LTS)
cd assets/admin
npm install
npm run build

# push the build
git add -A
git commit -m "Bump Version"
git push origin 2.6
```

Now also tag the skeleton:

```bash
bin/tag-release sulu/skeleton 2.6
```

Do the same for the upper version (3.0, 3.1, ...).
