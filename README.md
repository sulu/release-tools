# Release Tools

Tools used to create new release for our packages. Can be used for any Github Package to create release tags and changelogs.

## Tag a new Release

To tag a new version on a specific branch the following command can be used:

```bash
bin/tag-release sulu/sulu 2.x
```

## Generate Changelog

To generate a changelog for a new created tag the following command can be used:

```bash
bin/generate-changelog sulu/sulu 2.2.10...2.3.0
```

After the changelog is generated and all tests working correctly add the changelog
over the Github UI to the previous created tag.
