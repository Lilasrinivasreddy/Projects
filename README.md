SELECT
  fd.result_id AS result_id,
  'failure' AS status
FROM
  Tapsium_AIML.new_results_data AS fd
JOIN (
  SELECT result_id, MAX(date1) AS max_date
  FROM Tapsium_AIML.dummy_results_data
  GROUP BY result_id
) AS nd ON fd.result_id = nd.result_id AND fd.status = 'failure' AND nd.max_date = fd.date1

UNION ALL

SELECT
  sd.result_id AS result_id,
  'success' AS status
FROM
  Tapsium_AIML.new_results_data AS sd
JOIN (
  SELECT result_id, MAX(date1) AS max_date
  FROM Tapsium_AIML.dummy_results_data
  GROUP BY result_id
) AS nd ON sd.result_id = nd.result_id AND sd.status = 'success' AND nd.max_date = sd.date1;
