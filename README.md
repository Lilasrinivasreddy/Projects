SELECT Tapsium_AIML.dummy_results_data.result_id, Tapsium_AIML.dummy_results_data.id, Tapsium_AIML.dummy_results_data.date1, Tapsium_AIML.new_results_data.status
FROM Tapsium_AIML.dummy_results_data
INNER JOIN Tapsium_AIML.new_results_data
ON Tapsium_AIML.dummy_results_data.result_id = Tapsium_AIML.new_results_data.result_id
WHERE Tapsium_AIML.new_results_data.status IN ('failure', 'success')
ORDER BY Tapsium_AIML.dummy_results_data.date1 DESC;
