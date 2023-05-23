SELECT
  tap_res.result_id,
  tap_res.id,
  tap_res.date,
  tap_res_new.status
FROM tap_res
LEFT JOIN tap_res_new ON tap_res.result_id = tap_res_new.result_id
WHERE tap_res_new.status IS NOT NULL
GROUP BY tap_res.result_id, tap_res_new.status
ORDER BY tap_res.date DESC;
