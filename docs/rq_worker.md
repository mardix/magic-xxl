#webportfolio_extras.rq_worker

Creates a simple worker to add job, start worker etc

## Usage

    # config.py
    
    class Config(object):
        
        RQ_WORKER_URI = ""  # The redis uri
        RQ_WORKER_NAME = "default"  # The name of the worker
        RQ_WORKER_TTL = 600  # TTL
        RQ_WORKER_RESULT_TTL = 7200  # How long the results stay in
    
    
    # extras.py
    
    from webportfolio import WebPortfolio
    import webportfolio_extras.rq_worker
    
    rq_worker = webportfolio_extras.rq_worker.RQ_Worker()
    WebPortfolio.bind(rq_worker)
    
   
    # views.py
    
    import extras
            
    def hi():
        data = {}
        job = extras.rq_worker.add_job(callback, **data)
        job_id = job.id
        
    def callback(data):
        pass
        
    def my_job():
        job_id = "xxxxx"
        job = extras.rq_worker.get_job(job_id)
        
        
    # manager.py
    
    import webportfolio_extras.rq_worker
    
    rq_worker = webportfolio_extras.rq_worker.RQ_Worker(uri="***")
  
    # run the worker
    rq_worker.run()
    
    
    # If you already have a redis connection 
    
    import webportfolio_extras.rq_worker
    import redis 
    
    url = "redis://:pass@host:port/dbnumer"
    redis_db = redis.StrictRedis.from_url(url)
    
    rq_worker = webportfolio_extras.rq_worker.RQ_Worker(redis=redis_db)