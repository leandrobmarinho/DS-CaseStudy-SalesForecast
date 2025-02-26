### Created a separate script for processing test data 
A new file, 'process_test.py', was created to handle test data processing separately from the training data. We do not apply the filter of the last N days on the test data in order to maintain the complete historical record, ensuring that the forecasts are based on the full available history and accurately reflect the real-world scenario.


