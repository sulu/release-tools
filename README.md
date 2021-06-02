# Release Tools

Tools used to create new release for our packages.

## Tag a new Release

To tag a new version on a specific branch the following command can be used:

```bash
bin/release sulu/sulu 2.x
```

## Generate Changelog

To tag a new version on a specific branch the following command can be used:

```bash
bin/generate-changelog sulu/sulu 2.2.10...2.3.0
```

After the changelog is generated and all tests working correctly add the changelog
over the Github UI to the previous created tag.
