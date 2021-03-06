.DEFAULT_GOAL := all

FILES :=                                  \
    collatz-tests                         \
    Collatz.html                          \
    Collatz.log                           \
    Collatz.py                            \
    RunCollatz.in                         \
    RunCollatz.out                        \
    RunCollatz.py                         \
    TestCollatz.py

# uncomment these three lines when you've created those files
# you must replace GitHubID with your GitHubID
#    .travis.yml                           \
#    collatz-tests/GitHubID-RunCollatz.in  \
#    collatz-tests/GitHubID-RunCollatz.out \

.pylintrc:
	pylint --disable=locally-disabled --reports=no --generate-rcfile > $@

collatz-tests:
	git clone https://github.com/cs373t-spring-2018/collatz-tests.git

Collatz.html: Collatz.py
	-pydoc -w Collatz

Collatz.log:
	git log > Collatz.log

RunCollatz.pyx: Collatz.py RunCollatz.py .pylintrc
	-mypy   Collatz.py
	-pylint Collatz.py
	-mypy   RunCollatz.py
	-pylint RunCollatz.py
	./RunCollatz.py < RunCollatz.in > RunCollatz.tmp
	-diff RunCollatz.tmp RunCollatz.out

TestCollatz.pyx: Collatz.py TestCollatz.py .pylintrc
	-mypy     Collatz.py
	-pylint   Collatz.py
	-mypy     TestCollatz.py
	-pylint   TestCollatz.py
	-coverage run    --branch TestCollatz.py
	-coverage report -m

all:

check: $(FILES)

clean:
	rm -f  .coverage
	rm -f  .pylintrc
	rm -f  *.pyc
	rm -f  *.tmp
	rm -rf __pycache__
	rm -rf .mypy_cache

config:
	git config -l

docker:
	docker run -it -v $(PWD):/usr/collatz -w /usr/collatz gpdowning/python

format:
	autopep8 -i Collatz.py
	autopep8 -i RunCollatz.py
	autopep8 -i TestCollatz.py

run: RunCollatz.pyx TestCollatz.pyx

scrub:
	make clean
	rm -f  Collatz.html
	rm -f  Collatz.log
	rm -rf collatz-tests

status:
	make clean
	@echo
	git branch
	git remote -v
	git status

travis: collatz-tests Collatz.html Collatz.log
	make clean
	ls -al
	make run
	ls -al
	make -r check
