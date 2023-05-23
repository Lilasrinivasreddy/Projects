# Projects

SELECT trn.status, tr.id
FROM tap_res tr
JOIN tap_res_new trn ON tr.result_id = trn.result_id
WHERE (trn.status = 'failure' OR trn.status = 'success')
  AND tr.date = (
    SELECT MAX(date)
    FROM tap_res tr_inner
    WHERE tr_inner.result_id = tr.result_id
      AND tr_inner.date = (
        SELECT MAX(date)
        FROM tap_res tr_inner2
        WHERE tr_inner2.result_id = tr_inner.result_id
      )
  );
