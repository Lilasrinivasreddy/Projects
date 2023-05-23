SELECT
  d1.result_id,
  d1.status,
  d2.date1
FROM Tapsium_AIML.dummy_results_data d1
INNER JOIN Tapsium_AIML.new_results_data d2 ON d1.result_id = d2.result_id
WHERE d2.status IN ('failure', 'success')
ORDER BY d2.date1 DESC
LIMIT 3;


SELECT
  d.result_id,
  d.id,
  d.date1,
  n.status
FROM Tapsium_AIML.dummy_results_data AS d
INNER JOIN Tapsium_AIML.new_results_data AS n ON d.result_id = n.result_id
WHERE n.status IN ('failure', 'success')
ORDER BY d.date1 DESC;
