---
title: "Strengthening eBPF Security: Progress on Audit and Runtime Hardening"
url: "https://ebpf.foundation/strengthening-ebpf-security-progress-on-audit-and-runtime-hardening/"
date: "Fri, 10 Apr 2026 14:35:36 +0000"
author: "eBPF"
feed_url: "https://ebpf.foundation/feed/"
---
<p>This blog was originally posted at <a href="https://alpha-omega.dev/blog/strengthening-ebpf-security-progress-on-audit-and-runtime-hardening/">Alpha Omega</a></p>
<p><i>By Bill Mulligan, Cilium and eBPF Community Pollinator, Isovalent at Cisco and Board Member, eBPF Foundation</i></p>
<p>The eBPF Foundation’s Alpha-Omega engagement is helping advance security in two important and complementary ways: through independent expert review of critical eBPF execution paths, and through upstream engineering work to improve runtime memory-safety instrumentation for JITed eBPF programs.</p>
<p>eBPF plays an increasingly important role in modern Linux systems, enabling powerful capabilities for observability, networking, security, and performance analysis. That flexibility also raises the bar for correctness. The security model depends heavily on the kernel verifier, the assumptions it enforces, and the behavior of architecture-specific JIT compilers that translate eBPF programs into native machine code. Strengthening those layers is essential to preserving trust in the ecosystem.</p>
<p>With support from Alpha-Omega, the eBPF Foundation has been progressing on two parallel workstreams: an external security assessment led by STAR Labs, and implementation work to improve KASAN (Kernel Address Sanitizer) coverage, a dynamic memory safety error detector designed to find out-of-bounds and use-after-free bugs, for eBPF JIT programs.</p>
<h2><b>Independent assessment of verifier and JIT security</b></h2>
<p>One major focus of the engagement has been a deep security review of eBPF JIT backends and their interaction with the verifier.</p>
<p>The assessment covered the x86-64 and arm64 eBPF JIT compilers with focused analysis of the assumptions shared between the verifier and JIT backends. A key outcome from this phase is that no standalone security vulnerabilities were identified in the x86-64, arm64, or riscv64 JIT implementations themselves. That is an encouraging result, particularly given how central those paths are to production eBPF deployments.</p>
<p>At the same time, the review reinforced an important point: eBPF security depends at least as much on verifier correctness as on JIT correctness.</p>
<p>Across the work completed to date, the assessment identified 14 findings in total:</p>
<ul>
<li>8 high-risk findings</li>
<li>2 medium-risk findings</li>
<li>4 low-risk findings</li>
</ul>
<p>These findings include verifier bypass issues, memory and pointer leak problems, a race condition that can lead to memory corruption, and a denial-of-service issue affecting verifier behavior.</p>
<p>Two low-risk issues have already been fixed upstream. The remaining findings are proceeding through disclosure and remediation workflows.</p>
<p>This work has helped sharpen the community’s understanding of where the highest-impact security risks lie, while also providing concrete findings that can be addressed upstream.</p>
<h2><b>Advancing KASAN support for JITed eBPF programs</b></h2>
<p>The second major workstream has focused on improving KASAN-based memory-safety instrumentation for eBPF JIT programs.</p>
<p>This work began with architectural analysis: how JIT memory is allocated, how shadow memory is handled, and which classes of memory access require special treatment in JITed eBPF execution. That included attention to private stacks, helper-returned values, and map-backed memory regions.</p>
<p>From there, the work moved into prototyping.</p>
<p>A proof-of-concept test setup was developed using bpf_testmod kfuncs that return invalid or freed memory. That makes it possible to drive JITed eBPF programs into controlled bad-memory-access scenarios and validate how instrumentation should behave. This test work helped confirm that the most relevant instruction paths are the JIT-emitted load and store operations, particularly BPF_LD, BPF_LDX, BPF_ST, and BPF_STX.</p>
<p>That early prototyping created the foundation for implementation work now underway.</p>
<p>Progress to date includes:</p>
<ul>
<li>preparing and sending an RFC to the BPF mailing list;</li>
<li>introducing a new Kconfig gating model for the feature on x86-64;</li>
<li>adding initial JIT-side plumbing for direct calls to __asan_loadX and __asan_storeX; and</li>
<li>exploring verifier and aux-data changes so the JIT can distinguish stack accesses and emit checks more selectively.</li>
</ul>
<p>This work is still in development, and the current focus is on debugging early boot and runtime issues, refining stack-access instrumentation, and better understanding red-zone and poisoning behavior so the first upstreamable version can be scoped appropriately.</p>
<p>Even at this stage, the value is clear. Extending KASAN coverage into JITed eBPF execution helps close an important observability gap. It creates a path toward stronger runtime diagnostics for unsafe memory access in the areas where JIT translation and kernel execution intersect.</p>
<h2><b>Why this matters</b></h2>
<p>Taken together, these two workstreams show the value of combining independent security review with practical upstream engineering.</p>
<p>The audit work is surfacing real, validated findings in one of the most security-critical parts of the eBPF stack. That helps maintainers focus effort where it will matter most. Meanwhile, the KASAN work is beginning to build new defensive and diagnostic capabilities directly into the runtime path for JITed programs.</p>
<p>That combination is important. Security improvements are strongest when projects can both identify weaknesses and improve the tooling available to detect or constrain them in practice.</p>
<p>For the eBPF ecosystem, that means:</p>
<ul>
<li>better visibility into verifier-related risk;</li>
<li>stronger confidence in architecture-specific JIT implementations;</li>
<li>improved foundations for upstream hardening; and</li>
<li>better long-term support for memory-safety diagnostics in JITed execution paths.</li>
</ul>
<h2><b>What comes next</b></h2>
<p>The next phase of the Alpha-Omega engagement is expected to continue along both tracks.</p>
<p>On the assessment side, that means continued follow-up on disclosure and remediation, as well as deeper verifier-focused analysis where appropriate.</p>
<p>On the engineering side, that means advancing the JIT KASAN prototype toward a cleaner, upstream-ready implementation, with continued refinement around stack handling, instrumentation behavior, and feature scoping.</p>
<p>The work completed so far has already produced meaningful outcomes: concrete findings for remediation, confirmation about the state of the current JIT backends, and real implementation progress toward stronger runtime hardening.</p>
<p>The eBPF Foundation is grateful to Alpha-Omega for supporting this effort and helping the community invest in security work that is practical, technically deep, and directly relevant to critical open source infrastructure.</p>
<h3>About the Author</h3>
<p><img alt="" class="alignleft wp-image-1653 size-thumbnail" height="150" src="https://alpha-omega.dev/wp-content/uploads/sites/22/2026/04/bill-2-150x150.jpg" width="150" />Bill Mulligan is a cloud native pollinator and community builder. He has given talks, written articles, and appeared on podcasts on a wide range of topics around cloud native. While at CNCF he restarted the Kubernetes Community Day program. He is currently at Isovalent growing the Cilium and eBPF communities.</p>
