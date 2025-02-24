# Random Source

he source will generate random inputs with a specified pattern.

## Compile & deploy plugin

```shell
# cd $eKuiper_src
# go build -trimpath -modfile extensions.mod --buildmode=plugin -o plugins/sources/Random.so extensions/sources/random/random.go
# cp plugins/sources/Random.so $eKuiper_install/plugins/sources
```

Restart the eKuiper server to activate the plugin.

## Configuration

The configuration for this source is `$ekuiper/etc/sources/random.yaml`. The format is as below:

```yaml
default:
  interval: 1000
  seed: 1
  pattern:
    count: 50
  deduplicate: 0

ext:
  interval: 100

dedup:
  interval: 100
  deduplicate: 50
```
### Global configurations

Use can specify the global random source settings here. The configuration items specified in `default` section will be taken as default settings for the source when running this source.

### interval

The interval (ms) to issue a message.

### seed

The maximum integer to be produced by the random function

### pattern

The pattern to be generated by the source. In the above example, the pattern will be a json like {"count":50}

### deduplicate

An int value. If it is a positive number, the source will not issue the messages which are duplicates of any of the previous 'deduplicate' length of messages. If it is 0, the source won't check for duplications. If it is a negative number, the source will check for duplicates over any previous messages. Do not use negative length if you have very large input data sets as all the previous data will be kept.

## Override the default settings

If you have a specific connection that need to overwrite the default settings, you can create a customized section. In the previous sample, we create a specific setting named with `test`.  Then you can specify the configuration with option `CONF_KEY` when creating the stream definition (see [stream specs](../../../sqls/streams.md) for more info).

## Sample usage

```
demo (
		...
	) WITH (DATASOURCE="demo", FORMAT="JSON", CONF_KEY="ext", TYPE="random");
```

The configuration keys "ext" will be used.

