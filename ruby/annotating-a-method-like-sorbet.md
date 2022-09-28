# Annotating a method like Sorbet

Before [Sorbet](https://sorbet.org/), I had never seen Ruby methods being annotated or augmented in this way:

    class Example
        extend T::Sig

        def self.some_untyped_method
            nil
        end

        sig {params(x: Integer).returns(Integer)}
        def self.add_one(x)
            x + 1
        end
    end    
    
Without going too crazy, I wanted to understand how this might work, and came up with this:

    module Hooks
        def params(tag)
            @params = tag
        end

        def returns(tag)
            @returns = tag
        end

        def method_added(name)
            return unless @params || @returns
        
            params = @params
            @params = nil
            returns = @returns
            @returns = nil

            meth = instance_method(name)

            define_method(name) do |*args, &block|
                params.call(*args) if params
                ret = meth.bind(self).call(*args, &block)
                returns.call(ret) if returns
                ret
            end
        end
    end

By extending a class with the `Hooks` module, this makes it possible to annotate a method with `params` or `returns`. These annotations take a lambda as an argument, which is called with the parameters or return value of the method, respectively.

THis is how it is used:

    class Test
        extend Hooks

        params -> (*args) { puts "params: #{args}" }
        returns -> (val) { puts "returns: #{val}" }
        def test(foo)
            "You said #{foo}"
        end
    end

    test = Test.new
    test.test('abc')

Running this produces:

    params: ["abc"]
    returns: You said abc
