# 🧮 Point-to-Point Communication in MPI — Ping-Pong

<p align="center">
  <em>Didactic benchmark of inter-process communication with MPI, Python, and mpi4py.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/MPI-Message%20Passing%20Interface-blue?style=flat" alt="MPI">
  <img src="https://img.shields.io/badge/mpi4py-Python%20Interface-green?style=flat" alt="mpi4py">
  <img src="https://img.shields.io/badge/status-completed-brightgreen?style=flat" alt="Status: completed">
</p>

---

This project simulates **Point-to-Point** communication using **MPI** (*Message Passing Interface*) through the **mpi4py** library. The program runs the classic **Ping-Pong** experiment: one process sends a message to another process, which sends it back to the sender.

The experiment measures the round-trip time for different message sizes and calculates the transfer rate in MB/s. The results are exported to CSV for further analysis.

## 🎯 Objective

Demonstrate, through a practical and didactic example, how parallel processes communicate with MPI and how to measure the performance of that communication in terms of latency and bandwidth.

## ✨ Features

- Point-to-point communication between two MPI processes.
- Sending and receiving data with `Send()` and `Recv()`.
- Time measurement with `MPI.Wtime()`.
- Calculation of transferred volume and transfer rate.
- Generation of floating-point number arrays with NumPy.
- Export of results to a CSV file.

## 🛠 How to Use the Repository

📥 1. Clone the repository:

```bash
git clone https://github.com/jimmykiedis/Point2PointMPI.git
cd Point2PointMPI
```

You can also download the project as a ZIP file and extract it locally.

📋 2. Prerequisites

- Python 3.9 or newer
- An MPI implementation such as OpenMPI

🔗 3. Install MPI and project dependencies

On macOS:

```bash
brew install open-mpi
python -m pip install --upgrade pip
python -m pip install -e .
```

On Ubuntu/Debian:

```bash
sudo apt-get update
sudo apt-get install -y openmpi-bin openmpi-common libopenmpi-dev
python -m pip install --upgrade pip
python -m pip install -e .
```

▶️ 4. Run the program

Execute the benchmark with exactly two MPI processes:

```bash
mpirun -np 2 python mpi.py
```

The `-np 2` option is required because the program is designed for one sender process and one responder process.

At the end of the run, the program writes the collected measurements to `Resultados.csv`.

## 🏗️ Implementation Strategy

For each message size, the program generates an array of `n = 2^exp` `double` values, with `exp` ranging from 0 to 19. Rank 0 sends the array to process 1; process 1 then receives it and sends it back.

Process 0 measures the total interval between sending the message and receiving the response. With this value, the program calculates the volume transferred on the round trip (`n × 8 bytes × 2`) and obtains the transfer rate in MB/s. Only process 0 consolidates and saves the results to the CSV file.

## 🛠️ Technologies

| Technology | Use in the project |
| --- | --- |
| Python 3.x | Main language |
| MPI | Communication between processes |
| mpi4py | Python interface for MPI |
| NumPy | Generation of `double` value arrays |
| CSV | Export of measurements |

## 📁 Project Structure

```text
Projeto-MPI-PingPong/
├── mpi.py             # Main experiment code
├── Resultados.csv     # File generated with the measurements
└── README.md          # Project documentation
```

## 📝 Final Notes

- The order of messages shown in the terminal may vary, since the processes run concurrently.
- The program does not use artificial synchronization with `comm.Barrier()`, avoiding interference with the measurements.
- Latency corresponds to the round-trip communication time; the transfer rate represents the volume communicated per unit of time.
- The project was developed for didactic purposes in the Distributed Systems course.

## 📄 License

Academic project developed to study point-to-point communication in distributed systems.

The code may be consulted, studied, and adapted for educational purposes. To formally define the reuse terms, include a `LICENSE` file with the chosen license.

---

Developed by **Leonardo Farias** for the Distributed Systems course.
