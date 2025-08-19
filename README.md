---

# 🚀 FIFO Buffer (4-Entry) in Verilog

This repository contains the **design, implementation, and simulation** of a **4-entry FIFO (First-In-First-Out) Buffer** using **Verilog HDL**.
The project demonstrates how to build a FIFO like a professional digital design engineer—covering circuit thinking, RTL coding, and verification.

---

## 📌 Project Overview

A **FIFO Buffer** is a sequential memory structure that stores data in the order it arrives.

* **First-In → First-Out**
* Useful in **data pipelines, networking, UARTs, and processor communication**.

This design implements:

* **4 storage entries (4x8-bit configurable)**
* **Read & Write Pointers** for addressing
* **Full & Empty Flags** for flow control
* **Synchronous operation with a single clock**

---

## 🧠 Circuit Thinking (How We Built It Like a Pro)

1. **Storage**: A small memory array (4 words deep).
2. **Pointers**:

   * `wr_ptr` → points to where new data is written.
   * `rd_ptr` → points to where data is read.
3. **Control Logic**:

   * `fifo_full` → when all 4 entries are filled.
   * `fifo_empty` → when no data is available.
4. **Data Flow**:

   * Write: `data_in` stored at `wr_ptr`, pointer increments.
   * Read: `data_out` fetched from `rd_ptr`, pointer increments.

👉 Think of it as a **queue with a circular track** (pointers wrap around).

---

## 📐 Circuit Diagram
<img width="1536" height="1024" alt="ChatGPT Image Aug 19, 2025, 02_41_49 PM" src="https://github.com/user-attachments/assets/fc40b0e4-3b2e-4061-bbfd-0803d49a5f93" />


<img width="1387" height="894" alt="Screenshot from 2025-08-19 14-25-36" src="https://github.com/user-attachments/assets/c20a3ae2-64ec-47be-a96a-e74250658184" />


---

## 💻 Verilog Implementation

```verilog
`timescale 1ns / 1ps
`default_nettype none

module fifo #(
    parameter WIDTH = 8,
    parameter DEPTH = 4
)(
    input  wire              clk,
    input  wire              reset,
    input  wire              wr_en,
    input  wire              rd_en,
    input  wire [WIDTH-1:0]  data_in,
    output reg  [WIDTH-1:0]  data_out,
    output wire              fifo_full,
    output wire              fifo_empty
);

    reg [WIDTH-1:0] mem [0:DEPTH-1];
    reg [1:0] wr_ptr, rd_ptr;
    reg [2:0] count;

    assign fifo_full  = (count == DEPTH);
    assign fifo_empty = (count == 0);

    always @(posedge clk or posedge reset) begin
        if (reset) begin
            wr_ptr <= 0; rd_ptr <= 0; count <= 0; data_out <= 0;
        end else begin
            if (wr_en && !fifo_full) begin
                mem[wr_ptr] <= data_in;
                wr_ptr <= wr_ptr + 1;
                count <= count + 1;
            end
            if (rd_en && !fifo_empty) begin
                data_out <= mem[rd_ptr];
                rd_ptr <= rd_ptr + 1;
                count <= count - 1;
            end
        end
    end
endmodule
```

---

<img width="1854" height="1168" alt="Screenshot from 2025-08-19 14-39-54" src="https://github.com/user-attachments/assets/d4133ec5-b3f0-4210-b29c-472879232d45" />
<img width="1387" height="894" alt="Screenshot from 2025-08-19 14-29-01" src="https://github.com/user-attachments/assets/26b66f46-b88c-4f0a-a1a3-ecba5e891d1c" />

---

## ✅ Features

* 🔹 Parameterized width & depth
* 🔹 Full/Empty flag generation
* 🔹 Circular buffer logic (wrapping pointers)
* 🔹 Clean & synthesizable RTL
* 🔹 Verified with testbench simulations

---



## 🌟 Applications

* UART transmit/receive buffers
* Network packet queues
* Pipeline stage balancing
* CPU/GPU data synchronization

---

## 🤝 Contributions

This project is part of my **Verilog learning & portfolio building series**, alongside:

* ✅ 16x4 Synchronous RAM
* ✅ 16x4 Synchronous ROM
* ✅ UART Receiver
* ✅ Frequency Divider

👉 Check my GitHub for all digital design projects!

---

## 🔗 Author

👨‍💻 **Apratim Phadke**
Electronics & Telecommunication Engineer | VLSI Enthusiast
📌 Passionate about Digital Design, RTL, and STA

---

⚡ *“Building digital circuits is not just about code, it’s about thinking in hardware.”*

---


