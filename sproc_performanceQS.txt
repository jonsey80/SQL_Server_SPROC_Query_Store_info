select 
object_name(qsq.[object_id]) 'Query_object'
,qsp.plan_id
,qsp.query_plan
,min(qsrs.first_execution_time) 'first_execution_time'
,max(qsrs.last_execution_time) 'last_execution_time'
,max(qsrs.count_executions) 'count_executions'
,sum(qsrs.avg_cpu_time) 'avg_cpu_time'
,sum(qsrs.avg_logical_io_reads) 'avg_logical_io_reads'
,sum(qsrs.avg_logical_io_writes) 'avg_logical_io_writes'
,sum(qsrs.avg_physical_io_reads) 'avg_physical_io_reads'
,sum(qsrs.last_physical_io_reads) 'last_physical_io_reads'
,sum(qsrs.avg_query_max_used_memory) 'avg_query_max_used_memory'
,sum(qsrs.avg_page_server_io_reads) 'avg_page_server_io_reads'
--, qsws.wait_category_desc
--,qsws.avg_query_wait_time_ms
from sys.query_store_query qsq
inner join sys.query_store_query_text qst on qsq.query_text_id = qst.query_text_id
inner join sys.query_store_plan qsp on qsq.query_id = qsp.query_id 
inner join sys.query_store_runtime_stats qsrs on qsp.plan_id = qsrs.plan_id
--left outer join sys.query_store_wait_stats qsws on qsp.plan_id = qsws.plan_id
where qsq.[object_id] >0
group by qsq.[object_id]
,qsp.plan_id
,qsp.query_plan
order by sum(qsrs.last_physical_io_reads) desc

