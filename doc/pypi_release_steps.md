PYPI Test mssql-scripter upload
========================================

### Requirements:
1. Create and Register a user account on testpypi at [test pypi](https://testpypi.python.org/pypi?%3Aaction=register_form)

2.  Add `<clone root>` to your PYTHONPATH environment variable:
    ##### Windows
    ```BatchFile
    set PYTHONPATH=<clone root>;%PYTHONPATH%
    ```
    ##### OSX/Ubuntu (bash)
    ```Shell
    export PYTHONPATH=<clone root>:${PYTHONPATH}
    ```
3.	Install the dependencies:
    ```Shell
    python <clone root>/dev_setup.py clean
    ```


## Version Bump
 

	Versioning schema: {major}.{minor}.{patch}{release}{release_version}	
    Example: 1.0.0a0
To bump a particular segment of the version, From `<clone_root>` execute:

```Bash
versionbump major              ->  2.0.0a0
versionbump minor              ->  2.1.0a0
versionbump patch              ->  2.1.1a0
versionbump release            ->  2.0.0rc0
Versionbump release_version    ->  2.0.0rc1
```
**Note**: versionbump does not allow version bumping if your workspace has pending changes.This is to protect against any manual updates that may have been made which can lead toinconsistent versions across files. If you know what you are doing you can override this by appending --allow-dirty to the versionbump command.
	
## Build
1. Clean distribution folders:

    ##### Windows
      ```Batch
      rmdir /s dist
      rmdir /s mssqltoolsservice\dist
      ```
  
    ##### OSX/Ubuntu (bash)
      ```Shell
      rm -rf dist
      rm -rf mssqltoolsservice\dist
      ```
2. Build mssql-scripter source distribution, From `<clone root>` execute:
    ```BatchFile
    python setup.py sdist
    ```

3. Build mssqltoolsservice wheels for each supported platform, From `<clone root>/mssqltoolsservice` execute:
    ```BatchFile
	python buildwheels.py
	```

	Build a OS-Specific wheel:
	```BatchFile
    python buildwheels.py CentOS_7
    python buildwheels.py Debian_8
    python buildwheels.py Fedora_23
    python buildwheels.py openSUSE_13_2
    python buildwheels.py OSX_10_11_64
    python buildwheels.py RHEL_7
    python buildwheels.py Ubuntu_14
    python buildwheels.py Ubuntu_16
    python buildwheels.py Windows_7_64
    python buildwheels.py Windows_7_86
	```
4. Add a .pypirc configuration file:

    - Create a .pypirc file in your user directory:
        #### Windows: 
            Example: C:\Users\bob\.pypirc
		#### MacOS/Linux: 
            Example: /Users/bob/.pypirc
    
    - Add the following content to the .pypirc file, replace `your_username` and `your_passsword` with your account information created from step 1:
        ```BashFile
		[distutils]
		index-servers=
		    pypitest
		 
		[pypitest]
		repository = https://testpypi.python.org/pypi
		username = your_username
		password = your_password
        ```
4. Register and upload to pypi test server:
    
    
    
    ```BatchFile
    python register_upload.py register pypitest
    ```
    
    ```BatchFile
    python register_upload.py upload pypitest
    ```

5. Test install:

	**Note**: Specifying the test pypi server as the index to search for, pip will attempt to search for mssql-scripter's dependencies from the same server. This can result in a requirement not found error, but should not be a problem if dev_setup.py was ran during developer setup. If the error does occur, manually pip install the dependencies that are listed in setup.py and ensure the versions are correct.
	
	Install the mssql-scripter package that was just uploaded:
    ```BashFile
	pip install -i https://testpypi.python.org/pypi mssql-scripter
	```

	Upgrade to the mssql-scripter that was uploaded:
	```BashFile
    pip install --upgrade -i https://testpypi.python.org/pypi mssql-scripter
    ```