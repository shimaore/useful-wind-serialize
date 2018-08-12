Serialize calling the middleware function `name` in `cfg.use`.

    seem = require 'seem'

    serialize = seem (cfg,name) ->
      ctx = {cfg}
      yield serialize_modules cfg.use, ctx, name

    serialize_modules = seem (modules,ctx,name) ->
      errors = 0
      if modules?

        for m in modules when m[name]?
          debug "Calling middleware #{m.name}.#{name}()"
          ctx.__middleware_name = m.name ? '(unnamed middleware)'
          try
            yield m[name].call ctx, ctx
          catch error
            value = error.stack ? (try JSON.stringify error) ? error.toString()
            debug.dev "Middleware `#{m.name}.#{name}` failed", value
            errors++

      return errors

    serialize.modules = serialize_modules

    pkg = require './package.json'
    debug = (require 'tangible') pkg.name
    module.exports = serialize
