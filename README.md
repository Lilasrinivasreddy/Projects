SELECT
  result_id,
  id,
  date,
  status
FROM
  Tapsium_AIML.dummy_results_data
LEFT JOIN
  Tapsium_AIML.new_results_data
ON
  Tapsium_AIML.dummy_results_data.result_id = Tapsium_AIML.new_results_data.result_id
WHERE
  status = 'failure'
ORDER BY
  date DESC
LIMIT
  1
UNION ALL
SELECT
  result_id,
  id,
  date,
  status
FROM
  Tapsium_AIML.dummy_results_data
LEFT JOIN
  Tapsium_AIML.new_results_data
ON
  Tapsium_AIML.dummy_results_data.result_id = Tapsium_AIML.new_results_data.result_id
WHERE
  status = 'success'
ORDER BY
  date DESC
LIMIT
  1;
