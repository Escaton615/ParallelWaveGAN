PYTHON:= python3.6
CUDA_VERSION:= 10.0
PYTORCH_VERSION:= 1.4
DOT:= .
.PHONY: all clean

all: virtualenv.done

virtualenv.done:
	test -d venv || virtualenv -p $(PYTHON) venv
	. venv/bin/activate; pip install torch==$(PYTORCH_VERSION) \
		-f https://download.pytorch.org/whl/cu$(subst $(DOT),,$(CUDA_VERSION))/torch_stable.html
	. venv/bin/activate; cd ../; pip install -e .
	. venv/bin/activate; cd ../; pip install -e .[test]
	touch virtualenv.done

apex.done: virtualenv.done
	git clone https://github.com/NVIDIA/apex.git
	. venv/bin/activate; cd apex; \
		pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
	touch apex.done

clean:
	rm -fr venv apex *.done
	find -iname "*.pyc" -delete
