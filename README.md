# TCP Port Scanner

A beginner-friendly cybersecurity project focused on checking TCP ports on systems I own or have explicit permission to test. This project combines cybersecurity learning with quality assurance (QA): I will build the scanner in small steps and use tests to verify its behaviour, including how it handles invalid input and connection failures.

> **Project status:** Planning and requirements. Implementation and tests are not complete yet.

## Why I'm Building This

I want to strengthen my understanding of networking and cybersecurity by building a small tool, while applying QA practices to make its behaviour testable and reliable. This project is also an opportunity to practise writing clear requirements, automated tests, and useful documentation.

## Scope

The first version is intentionally small. It is planned to:

- Accept a target hostname or IP address.
- Accept a TCP port or a range of TCP ports.
- Validate port numbers and ranges before attempting connections.
- Attempt TCP connections using a configurable timeout.
- Report ports as **open**, **closed**, or **indeterminate** where the result cannot be determined reliably.
- Handle invalid input and connection errors with clear messages.
- Summarise the scan results.

### Out of Scope for the First Version

- Stealth scanning or evasion features.
- Service-version detection.
- Large-scale or internet-wide scanning.
- A graphical user interface or concurrent scanning.

## Responsible Use

This tool is for learning and authorised testing only. I will initially test against my own computer or a local test service that I control. I will not scan systems, networks, or devices belonging to other people or organisations without explicit permission and an appropriate scope.

## Planned Technology

- **Language:** Python
- **Networking:** Python's standard `socket` library
- **Testing:** `pytest`
- **Version control:** Git and GitHub

The exact setup instructions will be added after the first implementation is ready.

## Requirements

### Functional Requirements

1. Accept a target hostname or IP address.
2. Accept a valid TCP port or inclusive port range.
3. Reject port numbers outside `1â€“65535` and invalid ranges.
4. Validate inputs before any network connection is attempted.
5. Attempt a TCP connection to each selected port.
6. Report the observed state of each port.
7. Handle timeouts and connection errors without an unhandled crash.
8. Display a clear summary of the results.

### Non-Functional Requirements

- Keep the code beginner-friendly, readable, and maintainable.
- Use a configurable timeout so connection attempts do not wait indefinitely.
- Provide clear error messages.
- Keep the initial scan scope small and appropriate for authorised testing.
- Make important behaviour testable with automated tests.

## Planned Design

The initial design separates the work into four responsibilities:

1. **Input handling and validation** â€” check the target and port range.
2. **TCP scanner** â€” attempt a connection and interpret the result.
3. **Results reporting** â€” present the results and summary clearly.
4. **Automated tests** â€” verify expected behaviour and prevent regressions.

A successful TCP connection indicates that a port accepted the connection at the time of the test. A connection-refused result can indicate a closed port. A timeout or other network error may leave the state indeterminate; it should not automatically be labelled closed.

## QA Test Plan

These are planned test cases. They have **not been executed yet**.

| ID | Scenario | Expected result |
|---|---|---|
| TC01 | A valid single port is provided | The scanner attempts the selected port. |
| TC02 | A valid inclusive port range is provided | Every port in the range is checked. |
| TC03 | Port `0` is provided | Input is rejected before any connection attempt. |
| TC04 | Port `65536` is provided | Input is rejected before any connection attempt. |
| TC05 | Start port is greater than end port | The invalid range is rejected. |
| TC06 | Port input contains non-numeric text | Input is rejected with a clear message. |
| TC07 | A local test service accepts a TCP connection | The port is reported as open. |
| TC08 | A local port refuses a TCP connection | The port is reported as closed when the refusal result supports that conclusion. |
| TC09 | A connection attempt times out | The result is reported as timed out or indeterminate, not automatically closed. |
| TC10 | A hostname cannot be resolved | A clear error is reported without an unhandled crash. |
| TC11 | The start and end ports are the same valid port | Exactly one port is checked. |
| TC12 | Invalid port input is supplied | The TCP connection function is called zero times. |

### QA Approach

- **Unit testing:** test input validation and result classification independently.
- **Integration testing:** test the scanner against a local TCP service that I control.
- **Negative testing:** check invalid input, timeouts, and connection errors.
- **Regression testing:** rerun the tests after code changes to check that existing behaviour still works.
- **Test doubles/mocks:** where appropriate, mock the connection function to verify that invalid input causes zero connection attempts.

A displayed error message alone is not enough to prove that invalid input was handled safely. Tests should also verify that no network connection was attempted.

## Planned Repository Structure

```text
tcp-port-scanner/
â”œâ”€â”€ src/
â”‚   â””â”€â”€ port_scanner.py
â”œâ”€â”€ tests/
â”‚   â””â”€â”€ test_port_scanner.py
â”œâ”€â”€ docs/
â”‚   â””â”€â”€ test-plan.md
â”œâ”€â”€ README.md
â”œâ”€â”€ requirements.txt
â””â”€â”€ .gitignore
```

This structure is a plan and may be adjusted as the project develops.

## Development Roadmap

- [x] Define the problem and project scope.
- [x] Draft functional and non-functional requirements.
- [x] Plan the first features and QA test cases.
- [ ] Implement and test port input validation.
- [ ] Implement a single TCP connection check.
- [ ] Scan an inclusive port range.
- [ ] Add timeout and error handling.
- [ ] Add automated unit tests with `pytest`.
- [ ] Add integration tests using a local test service.
- [ ] Record actual test results and fix defects.
- [ ] Add setup and usage instructions.
- [ ] Add automated test execution in a CI pipeline when practical.

## Current Progress

**Completed:** initial requirements, feature plan, and draft QA test plan.

**Next:** design the input-validation behaviour and write tests for valid and invalid port numbers before implementing the scanner.

## Learning Notes

This section will record what I learn about TCP, sockets, connection outcomes, input validation, and QA as the project progresses. I will update it based on work I have actually completed.

## Disclaimer

This project is an educational tool, not a guarantee of security. Scan results can be affected by firewalls, network conditions, permissions, and changes in the target service. Testing should remain within an explicitly authorised scope.
