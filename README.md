# benchmarks
MLOps Engines, Frameworks, and Languages benchmarks over main stream AI Models.

## Structure

The repository is organized to facilitate benchmark management and execution through a consistent structure:

- Each benchmark, identified as `bench_name`, has a dedicated folder, `bench_{bench_name}`.
- Within these benchmark folders, a common script named `bench.sh` handles setup, environment configuration, and execution.

### Benchmark Script

The `bench.sh` script supports key parameters:

- `prompt`: Benchmark-specific prompt.
- `max_tokens`: Maximum tokens for the benchmark.
- `repetitions`: Number of benchmark repetitions.
- `log_file`: File for storing benchmark logs.
- `device`: Device for benchmark execution (cpu, cuda, metal).
- `models_dir`: Directory containing necessary model files.

### Unified Execution

An overarching `bench.sh` script streamlines benchmark execution:

- Downloads essential files for benchmarking.
- Iterates through all benchmark folders in the repository.

This empowers users to seamlessly execute benchmarks based on their preference. To run a specific benchmark, navigate to the corresponding benchmark folder (e.g., `bench_{bench_name}`) and execute the `bench.sh` script with the required parameters.



## Usage

```bash
# Run a specific benchmark
./bench_{bench_name}/bench.sh --prompt <value> --max_tokens <value> --num_repetitions <value> --log_file <file_path> --device <cpu/cuda/metal> --models_dir <path_to_models>

# Run all benchmarks collectively
./bench.sh --prompt <value> --max_tokens <value> --num_repetitions <value> --log_file <file_path> --device <cpu/cuda/metal> --models_dir <path_to_models>
```


## ML Engines: Feature Table

| Features                    | pytorch | burn | llama.cpp | candle | tinygrad | onnxruntime | CTranslate2 |
| --------------------------- | ------- | ---- | --------- | ------ | -------- | ----------- | ----------- |
| Inference support           | ✅      | ✅   | ✅        | ✅     | ✅       | ✅          | ✅          |
| 16-bit quantization support | ✅      | ✅   | ✅        | ✅     | ✅       | ✅          | ✅          |
| 8-bit quantization support  | ✅      | ❌   | ✅        | ✅     | ✅       | ✅          | ✅          |
| 4-bit quantization support  | ✅      | ❌   | ✅        | ✅     | ❌       | ❌          | ❌          |
| 2/3bit quantization support | ✅      | ❌   | ✅        | ✅     | ❌       | ❌          | ❌          |
| CUDA support                | ✅      | ✅   | ✅        | ✅     | ✅       | ✅          | ✅          |
| ROCM support                | ✅      | ✅   | ✅        | ✅     | ✅       | ❌          | ❌          |
| Intel OneAPI/SYCL support   | ✅**    | ✅   | ✅        | ✅     | ✅       | ❌          | ❌          |
| Mac M1/M2 support           | ✅      | ✅   | ✅        | ⭐     | ✅       | ✅          | ⭐          |
| BLAS support(CPU)           | ✅      | ✅   | ✅        | ✅     | ❌       | ✅          | ✅          |
| Model Parallel support      | ✅      | ❌   | ❌        | ✅     | ❌       | ❌          | ✅          |
| Tensor Parallel support     | ✅      | ❌   | ❌        | ✅     | ❌       | ❌          | ✅          |
| Onnx Format support         | ✅      | ✅   | ✅        | ✅     | ✅       | ✅          | ❌          |
| Training support            | ✅      | 🌟   | ❌        | 🌟     | ❌       | ❌          | ❌          |

⭐ = No Metal Support
🌟 = Partial Support for Training (Finetuning already works, but training from scratch may not work)

## Benchmarking ML Engines

### A100 80GB Inference Bench:

Model: LLAMA-2-7B

CUDA Version: 11.7

Command: `./benchmark.sh --repetitions 10 --max_tokens 100 --device gpu --nvidia --prompt 'Explain what is a transformer'`

| Engine      | float32      | float16       | int8          | int4          |
|-------------|--------------|---------------|---------------|---------------|
| burn        | 13.12 ± 0.85 |      -        |      -        |      -        |
| candle      |      -       | 36.78 ± 2.17  |      -        |      -        |
| llama.cpp   |      -       |      -        | 84.48 ± 3.76  | 106.76 ± 1.29 |
| ctranslate  |      -       | 51.38 ± 16.01 | 36.12 ± 11.93 |      -        |
| tinygrad    |      -       | 20.32 ± 0.06  |      -        |      -        |

*(data updated: 23th November 2023)


### M2 MAX 32GB Inference Bench:

#### CPU

Model: LLAMA-2-7B

CUDA Version: NA

Command: `./benchmark.sh --repetitions 10 --max_tokens 100 --device cpu --prompt 'Explain what is a transformer'`

| Engine      | float32       | float16       | int8         | int4         |
|-------------|--------------|--------------|--------------|--------------|
| burn        | 0.30 ± 0.09  |      -       |      -       |      -       |
| candle      |      -       | 3.43 ± 0.02  |      -       |      -       |
| llama.cpp   |      -       |      -       | 14.41 ± 1.59 | 20.96 ± 1.94 |
| ctranslate  |      -       |      -       | 2.11 ± 0.73  |      -       |
| tinygrad    |      -       | 4.21 ± 0.38  |      -       |      -       |

#### GPU (Metal)

Command: `./benchmark.sh --repetitions 10 --max_tokens 100 --device gpu --prompt 'Explain what is a transformer'`

| Engine      | float32       | float16       | int8         | int4         |
|-------------|--------------|--------------|--------------|--------------|
| burn        |      -       |      -       |      -       |      -       |
| candle      |      -       |      -       |      -       |      -       |
| llama.cpp   |      -       |      -       | 31.24 ± 7.82 | 46.75 ± 9.55 |
| ctranslate  |      -       |      -       |      -       |      -       |
| tinygrad    |      -       | 29.78 ± 1.18 |      -       |      -       |

*(data updated: 23th November 2023)
