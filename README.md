# LogStash_Exercise

# Phases of Exercise
1. Logstash installation
-> for this exercise, only logstash installation was done from the ELK stack

2. Input files for logstash
-> there are 2 files:
    - test.log (contains security log from the original question in context)
    - test2.log (contains modified security log with similar structure but different data in it)

3. Logstash configuration file
-> Please refer to the files/config_files folder for logstash_config.conf file
-> this single config file contains data for all the 3 pipelines: input, filter and output.

4. screenshots
there are 4 screenshots in total:
-> input_log_left_config_file_right.png = this screenshot shows the test.log file on left and the config file used on the right side.
-> stdout_logstash_left_file_output_right.png = this screenshot shows the logstash live output on the left (due to stdout as one of the output plugins) as well as the output.json file on the right, actively written by logstash.
-> input_log_left_config_file_right_CASE_2.png = this screenshot shows the test2.log file (modified log file data-wise, structure same) on left and the config file used on the right side.
-> stdout_logstash_left_file_output_right_CASE_2.png = this screenshot shows the logstash live output on the left (due to stdout as one of the output plugins) as well as the output2.json file on the right, actively written by logstash.

5. Output files for logstash
-> there are 2 files:
    - output.json
    - output2.json

-------------------------------
# Explaination for 3 phases of config file

1. input phase
-> the file input option was used as input method (test.log and test2.log)

2. filter phase
-> AIM: normalization of data before exporting it via output phase.
steps for plugins used:
    
    - grok plugin: with this one, half of the logs are filtered via known regex patterns and other half, which is more compliant to KV plugin (key=value pairs), is passed to the next plugin as input (field name is remaining)

    - kv plugin: this one takes remaining field as input and splits the fields as shown

    - mutate plugin: renaming 4 field names as per required in question

    - mutate plugin: changing datatype of 3 fields

    - mutate plugin: replacing \" in source_ip with empty string

    - if else conditionals: to add a new field named severity corresponding to the numerical value given in the log.
    assumptions were made for the extra case as follows:
    1 = high (mentioned in the question)
    2 = critical
    else = unknown 

    - prune plugin: to remove unnecessary fields from the output.

3. output phase
consists of 2 plugins:
-> stdout: to observe output for every logstash parsing in terminal.
-> file: exports the parsed results to output.json / output2.json files.

-----------------------------
# commands used

/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash_config.conf --config.reload.automatic