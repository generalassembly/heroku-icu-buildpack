# Heroku ICU Buildpack

A buildpack for including the ICU library into a Heroku dyno. This is required
as a dependency for several tools.

## How it works

This buildpack makes ICU 4.8 available in the `/app/icu` directory for both
compilation and runtime uses. The primary purpose of this is to allow Charlock
Holmes (a ruby gem for encoding heuristics) to run on Heroku.

In order to achieve this, the following happens:

1. ICU is compiled from the vendored version under `icu-src` inside this
   buildpack
2. This is cached for future use in Heroku's cache directory
3. The compiled result is copied into `/app/icu` and `$1/icu` (the build
   directory) to make it available in a seemingly consistent location during
   both compilation and runtime
4. A bundler config option is exported to the rest of the compilation process to
   ensure Charlock Holmes looks for ICU in the correct place.
