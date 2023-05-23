SELECT
  fd.result_id AS result_id,
  'failure' AS status
FROM
  Tapsium_AIML.new_results_data AS fd
JOIN (
  SELECT result_id, MAX(date1) AS max_date
  FROM Tapsium_AIML.dummy_results_data
  GROUP BY result_id
) AS nd ON fd.result_id = nd.result_id AND fd.status = 'failure'
JOIN Tapsium_AIML.dummy_results_data AS tr ON tr.result_id = fd.result_id AND tr.date1 = nd.max_date

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
) AS nd ON sd.result_id = nd.result_id AND sd.status = 'success'
JOIN Tapsium_AIML.dummy_results_data AS ts ON ts.result_id = sd.result_id AND ts.date1 = nd.max_date;
