1. **Environment Setup**

   - Install Python 3.11, Maven 3.9.6, Java 8, Postgres 16, and configure the corresponding environment.

   - Install Python library dependencies

     ```
     pip install -r requirements.txt
     ```

   - Set Prefect parameters

     ```
     prefect config set PREFECT_API_DATABASE_CONNECTION_URL="postgresql+asyncpg://postgres:yourPassWord@localhost:5432/prefect"	
     prefect config set PREFECT_API_URL="http://127.0.0.1:4200/api"
     prefect config set PREFECT_UI_API_URL="http://127.0.0.1:4200/api"
     ```

2. **Usage**

   - The Config in the file ./utils/constants.py defines the execution path for unit tests at runtime, for example:

     ```
     maven_project: List[str] = [
             f"{PROJECT_ROOT}/project/TestJavaCode"
             # f"{PROJECT_ROOT}/project/TestJavaCode2",
             # f"{PROJECT_ROOT}/project/TestJavaCode3",
         ]
     ```

     

   -  This path should be a Maven project containing a `pom.xml` file, which should include dependencies for dataset compilation and testing (such as JUnit, Clover, etc.).

   - Start the Prefect backend service

     ```
     prefect server start
     ```

   - Modify the parameters in main.py and run main.py

     ```
     python main.py
     ```

     Parameter details are as follows:

     | Parameter           | Type         | Description                                                  |
     | ------------------- | ------------ | ------------------------------------------------------------ |
     | mode                | CoverageMode | Metric for running unit tests in the system, divided into branch coverage and line coverage. |
     | llm_config          | LLMConfig    | Parameters for setting up the large model.                   |
     | no_code_reply       | str          | Feedback message to the large model when no code output is detected by the system. |
     | syntax_error_reply  | str          | Feedback message to the large model when code output contains syntax errors. |
     | max_retry           | int          | Maximum number of retries for fixing code runtime errors.    |
     | max_iteration       | int          | Maximum number of feedback iterations when code does not meet coverage requirements. |
     | max_fix             | int          | Maximum number of retries for template fixes when code runtime errors occur. |
     | auto_fix            | bool         | Enable template fixes.                                       |
     | fix_compile_error   | bool         | Enable compilation error fixes.                              |
     | cache_key           | str          | Cache key for the output of the large model. When cache_key and prompts are the same, the output of the large model will be directly read from the cache. |
     | dataset_name        | str          | Name of the dataset for generating unit test tasks, same as the file name. |
     | dataset_size        | int          | Number of unit test generation tasks to stop after.          |
     | dataset_start_index | int          | Start generating tasks from which data in the dataset.       |

3. **Custom Dataset**

   - Run` \ui\main.py`, call the `upload_file` method using an HTTP tool, and upload a zip file. The zip file should only contain the `\src\main\java` code directory:

     `-src`

     `|_ main`

     ​	`|_ java`

     The processed dataset will be saved in `\dataset\datasetname_processed.zip`.

4. **Others**

   - Code and other files generated in the TestART experiment: [Link to files](https://pan.baidu.com/s/1S0KSdtdW7R6unYnoTEe5XA?pwd=tfn7)
