# Flabongo 3rd Party Libs

Custom [jQuery](https://jquery.com/) and [Lodash](https://lodash.com/) libraries for GetFeedback Direct Ruby+JS.
To reduce library sizes, only functions actually used in the project are included.

## Usage

Add `flabongo-3rd-party-libs` as a dependency in your Ruby project's `package.json`, e.g.:

```json5
{
  "dependencies": {
    "flabongo-3rd-party-libs": "github:MatchbookLabs/flabongo-3rd-party-libs#2.0.0"
  }
}
```

During your project build, copy the `flabongo-3rd-party-libs` contents to somewhere your published app will find a `require`d module, e.g. `vendor/assets/javascripts/embed`.

```shell
cp ${root_dir}/node_modules/flabongo-3rd-party-libs/lib/jquery.js ${javascripts_embed_dir}/jquery.js
cp ${root_dir}/node_modules/flabongo-3rd-party-libs/lib/lodash.js ${javascripts_embed_dir}/lodash.js
```

Include these packages in the Ruby asset precompile. (`assets.rb`)

```ruby
Rails.application.config.assets.precompile +=
  [
    ...
   'embed/jquery.js',
   'embed/lodash.js',
    ...
  ]
```

Set up well-known global variables, e.g. in `js.js.erb`:

```erbruby
var _ = <%= inline_asset 'embed/lodash.js' %>

...

<%= inline_asset 'embed/jquery.js' %>
var $ = gf.$ = module.exports;
```

Now the functions of JQuery and Lodash are available via `$` and `_`, respectively.

## Development

To develop locally:

1. Copy the repo to your machine.
2. Install dependencies
    ```shell
    yarn install
    ```
3. The custom builds are created by the `bin/npm-build` script that you can invoke using `yarn`.
   ```shell
   yarn build
   ```

### Updating `yarn` and `lodash`

To upgrade the versions of the packages being customized:

1. Modify the `dependencies` in `package.json`.
2. Install dependencies and build, as shown earlier.
3. Update the `version` in `package.json`, following semantic versioning.
   (The new version number choice depends on the nature of the inherited changes, see [When your Public API is in my Public API](https://github.com/semver/semver/issues/80).
   Since we're producing a subset of the 3rd-party packages containing old, stable APIs, our updates rarely see an API change are are therefore patches.)
4. Check in the updated `package.json`, `lib/*`, and `yarn.lock` files.

### Publishing new versions

Nothing automated here; when you've completed the work described above, ...

5. Once merged to the main branch, tag the commit with a `vX.X.X` tag.

Clients can now update their `dependencies` to pick up the new version directly from Github.

## License

Both jQuery and Lodash use the [MIT](https://choosealicense.com/licenses/mit/) license.
