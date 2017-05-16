Serialize calling the middleware function `name` in `cfg.use`.

    seem = require 'seem'

    serialize = seem (cfg,name) ->

      ctx = {cfg}
      if cfg.use?

        for m in cfg.use when m[name]?
          debug "Calling middleware #{m.name}.#{name}()"
          ctx.__middleware_name = m.name ? '(unnamed middleware)'
          yield m[name].call ctx, ctx

    pkg = require './package.json'
    debug = (require 'tangible') "#{pkg.name}:serialize"
    module.exports = serialize
