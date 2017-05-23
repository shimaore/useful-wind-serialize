Serialize calling the middleware function `name` in `cfg.use`.

    seem = require 'seem'

    serialize = seem (cfg,name) ->
      ctx = {cfg}
      serialize_modules cfg.use, ctx, name

    serialize_modules = (modules,ctx,name) ->
      if modules?

        for m in modules when m[name]?
          debug "Calling middleware #{m.name}.#{name}()"
          ctx.__middleware_name = m.name ? '(unnamed middleware)'
          it = yield m[name].call ctx, ctx

      it

    serialize.modules = serialize_modules

    pkg = require './package.json'
    debug = (require 'tangible') pkg.name
    module.exports = serialize
