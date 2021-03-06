Bae
===

Bae is a multinomial naive bayes classifier based on another gem ["naivebayes"](https://github.com/id774/naivebayes), only this one uses java to do the heavy lifting.

By default this will use the vanilla ruby implementation, but you can use the native classifier written in java.

```ruby
require 'bae/native_classifier'

classifier = ::Bae::NativeClassifier.new
```


## Installation

Add this line to your application's Gemfile:

    gem 'bae'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install bae

## Usage

You can refer to ["naivebayes"](https://github.com/id774/naivebayes) gem for more documentation, or the tests for examples. Here is a copy/ paster example:


### You can provide a frequency hash to the trainer

```ruby
classifier = ::Bae::Classifier.new
classifier.train("positive", {"aaa" => 0, "bbb" => 1})
classifier.train("negative", {"ccc" => 2, "ddd" => 3})

classifier.finish_training!

classifier.classify({"aaa" => 1, "bbb" => 1})

#=> {"positive" => 0.8767123287671234, "negative" => 0.12328767123287669}
```

### Or you can train with strings
```ruby
classifier = ::Bae::Classifier.new
classifier.train("positive", "aaa aaa bbb");
classifier.train("negative", "ccc ccc ddd ddd");
classifier.train("neutral", "eee eee eee fff fff fff");

classifier.finish_training!

classifier.classify("aaa bbb")

#=> {"positive"=>0.8962655601659751, "negative"=>0.0663900414937759, "neutral"=>0.037344398340248955}
```

### Saving State

You can actually save a snapshot of the trained classifier to disk and load it into memory.

```ruby
# From the example above...
classifier = ::Bae::Classifier.new
classifier.train("positive", {"aaa" => 0, "bbb" => 1})
classifier.train("negative", {"ccc" => 2, "ddd" => 3})

classifier.finish_training!

classifier.classify({"aaa" => 1, "bbb" => 1})
#=> {"positive" => 0.8767123287671234, "negative" => 0.12328767123287669}

# Now let's save it to disk
classifier.save_state("/tmp/some_state.json")

# Let's create a new classifier and load from the sate we just saved
classifier = ::Bae::Classifier.new
classifier.load_state("/tmp/some_state.json")

# Now we can classify without retraining
classifier.classify({"aaa" => 1, "bbb" => 1})
#=> {"positive" => 0.8767123287671234, "negative" => 0.12328767123287669}
```


## Contributing

1. Fork it ( https://github.com/film42/bae/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
