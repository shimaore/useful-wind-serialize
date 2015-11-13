    chai = require 'chai'
    chai.should()

    describe 'Serialize', ->
      serialize = require '..'
      it 'should run in order', (done) ->
        mw1 = 
          include: ->
            @a = 3
        mw2 =
          include: ->
            @a.should.equal 3
            done()
        serialize use: [mw1, mw2], 'include'
