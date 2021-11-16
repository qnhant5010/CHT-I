# CHT-I
The dataset used for benchmarking the model of chemotherapy service, based on historical data of Centre Hospitalier de Troyes (France, 2019).

The folder `dat` is for CPLEX. The folder `json` is for other programming language, such as Java.

# Problem description

The outpatient scheduling at the chemotherapy service includes two subproblems. In the first problem with time horizon considered by the number of days, the scheduler must decide the starting time of a sequence of `treatment sessions` (as known as `appointment`s) and rest days in-between. The second planning problem has the time horizon limited by a day and consists of a four-stage process requiring multiple resources at each stage.

The first sub-problem considers, for each patient, a complete chemotherapy outpatient therapy, which is called the `multi-day pattern`. The therapy comprises several treatment sessions (i.e., appointments) separated by predefined resting time windows for healing. These inter-appointment periods are fixed so that the patient's health should be ready for the next session. For the sake of brevity, this sequence is denoted as a `cycle` of a patient.

The second sub-problem handles the scheduling on the day of the appointment, decided by the first subproblem. After being admitted, the patient consults a doctor to check up on her/his current health condition. If the patient is judged not to receive the treatment, the current session will be rescheduled to another day. Otherwise, the patient follows a nurse to an available seat (a bed or an armchair). In parallel, the  required chemotherapy drug is prepared at the pharmacy unit. Once the drug is ready, the patient proceeds to receive the treatment under the monitoring of a nurse.

However, some cases do not require consultations, while others allow the consultation before going to the service. In those special cases, the consulting time of the patient is considered to be null. Furthermore, occasionally, the mixing drug step is permitted to proceed _one day in advance_ so that the waiting time can be reduced.

# Data description

Parameter | Type | Description
--- | --- | ---
O = \{A, B, C, D\} | Set | 4 operations of an appointments: consultation, installation, drug mixing, monitoring
N_J | int | Number of days
N_H | int | Number of time-slots per day
N_D | int | Number of sectors
D | int[d][j][h] | Number of doctors of sector `d` at day `j` time `h`
I | int[j][h] | Number of nurses at day `j` time `h`
F | boolean[j][h] | 1 if pharmacy unit opens at day `j` time `h`; 0 otherwise
y | int | Number of patients that a nurse can monitor simultaneously
a | int | Duration of consultation
b | int | Duration of installation
M | int | Number of seats
N_P | int | Number of patients
n | int[p] | Number of appointments of patient `p`
Delta | int[p][k] | The number of rest-days of patient `p` before appointment `k`
e_A | boolean[p][k] | 1 if patient `p` needs consultation at appointment `k`; 0 otherwise
e_C | boolean[p][k] | 1 if patient `p` can have drug mixing prepared in advance; 0 otherwise
om | int[p][s] | 1 if patient `p` requires doctor of sector `s`; 0 otherwise
p_C | int[p] | Duration of drug mixing for patient `p`
p_D | int[p] | Duration of monitoring for patient `p`