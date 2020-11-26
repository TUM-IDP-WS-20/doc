# Google Colab

## Issues
1. ### `The exit codes of the workers are {EXIT(1)}` error when LatentDirichletAllocation.fit_transform called

- Code:

```python
from sklearn.decomposition import LatentDirichletAllocation
import pyLDAvis
import pyLDAvis.sklearn

count_vectorizer = CountVectorizer(analyzer='word',       
                             min_df=3,                       
                             stop_words='english',             
                             lowercase=True,                   
                             token_pattern='[a-zA-Z0-9]{3,}',  
                             max_features=5000,          
                            )

data_vectorized = count_vectorizer.fit_transform(dt3c['data'])

lda_model = LatentDirichletAllocation(n_components=NUM_OF_TOPICS, # Number of topics
                                      learning_method='online',
                                      random_state=0,       
                                      n_jobs = -1  # Use all available CPUs
                                     )
lda_output = lda_model.fit_transform(data_vectorized)

pyLDAvis.enable_notebook()
pyLDAvis.sklearn.prepare(lda_model, data_vectorized, count_vectorizer, mds='tsne')
```

- Error:

```python
---------------------------------------------------------------------------
TerminatedWorkerError                     Traceback (most recent call last)
<ipython-input-30-9fa0a56b57b7> in <module>()
     18                                       n_jobs = -1  # Use all available CPUs
     19                                      )
---> 20 lda_output = lda_model.fit_transform(data_vectorized)
     21 
     22 pyLDAvis.enable_notebook()

8 frames
/usr/lib/python3.6/concurrent/futures/_base.py in __get_result(self)
    382     def __get_result(self):
    383         if self._exception:
--> 384             raise self._exception
    385         else:
    386             return self._result

TerminatedWorkerError: A worker process managed by the executor was unexpectedly terminated. This could be caused by a segmentation fault while calling the function or by an excessive memory usage causing the Operating System to kill the worker.

The exit codes of the workers are {EXIT(1)}
```

- Solution:
As mentioned <a href="https://stackoverflow.com/questions/54139403/how-do-i-fix-debug-this-multi-process-terminated-worker-error-thrown-in-scikit-l">here</a>, it might caused by 
  - Sufficient memory(RAM)
  - Incompatible library versions.
  
We've checked memory consumption and try to increase memory of GColab as mentioned <a href="https://github.com/googlecolab/colabtools/issues/253#issuecomment-551056637">here</a>. However, the problem is not related with the memory usage. If the memory is not enought, Google Colab pops up a notification to inducate that there is not enough memory to continue then it restart the environment.

We don't know the root cause, but simply removing `n_jobs = -1` parameters or set it to default `n_jobs = None`, solve this problem.

However, when we remove this parameter, it throws another similar error for `pyLDAvis.sklearn.prepare(lda_model, data_vectorized, count_vectorizer, mds='tsne')` that we showed below.

2. ### `The exit codes of the workers are {EXIT(1), EXIT(1)}` error when LatentDirichletAllocation.fit_transform called

- Code shown in 1 the only difference is setting `n_jobs = None`
- Error:

```python
exception calling callback for <Future at 0x7ff28ec3ccf8 state=finished raised TerminatedWorkerError>
Traceback (most recent call last):
  File "/usr/local/lib/python3.6/dist-packages/joblib/externals/loky/_base.py", line 625, in _invoke_callbacks
    callback(self)
  File "/usr/local/lib/python3.6/dist-packages/joblib/parallel.py", line 366, in __call__
    self.parallel.dispatch_next()
  File "/usr/local/lib/python3.6/dist-packages/joblib/parallel.py", line 799, in dispatch_next
    if not self.dispatch_one_batch(self._original_iterator):
  File "/usr/local/lib/python3.6/dist-packages/joblib/parallel.py", line 866, in dispatch_one_batch
    self._dispatch(tasks)
  File "/usr/local/lib/python3.6/dist-packages/joblib/parallel.py", line 784, in _dispatch
    job = self._backend.apply_async(batch, callback=cb)
  File "/usr/local/lib/python3.6/dist-packages/joblib/_parallel_backends.py", line 531, in apply_async
    future = self._workers.submit(SafeFunction(func))
  File "/usr/local/lib/python3.6/dist-packages/joblib/externals/loky/reusable_executor.py", line 178, in submit
    fn, *args, **kwargs)
  File "/usr/local/lib/python3.6/dist-packages/joblib/externals/loky/process_executor.py", line 1102, in submit
    raise self._flags.broken
joblib.externals.loky.process_executor.TerminatedWorkerError: A worker process managed by the executor was unexpectedly terminated. This could be caused by a segmentation fault while calling the function or by an excessive memory usage causing the Operating System to kill the worker.

The exit codes of the workers are {EXIT(1), EXIT(1)}
---------------------------------------------------------------------------
TerminatedWorkerError                     Traceback (most recent call last)
<ipython-input-20-4368cc5ea5a0> in <module>()
----> 1 pyLDAvis.sklearn.prepare(lda_model, data_vectorized, count_vectorizer, mds='tsne')

15 frames
/usr/local/lib/python3.6/dist-packages/joblib/externals/loky/process_executor.py in submit(self, fn, *args, **kwargs)
   1100         with self._flags.shutdown_lock:
   1101             if self._flags.broken is not None:
-> 1102                 raise self._flags.broken
   1103             if self._flags.shutdown:
   1104                 raise ShutdownExecutorError(

TerminatedWorkerError: A worker process managed by the executor was unexpectedly terminated. This could be caused by a segmentation fault while calling the function or by an excessive memory usage causing the Operating System to kill the worker.

The exit codes of the workers are {EXIT(1), EXIT(1)}
```

- Solution
Try: pyLDAvis.sklearn.prepare(...) uses n_jobs=-1 as default as shown <a href="https://pyldavis.readthedocs.io/en/latest/modules/API.html">here</a>. Changing this parameter may fix the problem.
