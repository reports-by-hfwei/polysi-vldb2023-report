% backup.md

%%%%%%%%%%%%%%%%%%%%
% \begin{frame}{Snapshot Isolation (SI)}
%   \begin{center}
%     \fig{width = 0.90\textwidth}{figs/si-hierarchy}
%   \end{center}
% \end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
% \begin{frame}{SI Prevents the ``Lost Update'' Anomaly}
%   \begin{center}
%     \input{tikz/banking-lost-update-tikz}

%     \uncover<3->{
%       \vspace{0.50cm}
%       $T_{A}$ and $T_{B}$ are executed concurrently.
%     }
%   \end{center}
% \end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Contribution: the \polysi{} Checker}
  \begin{center}
    % \uncover<2->{
    % Sound and complete \polysi{} outperformed state-of-the-art SI checkers \\[2pt]
    % and scales up to large workloads.
    % }

    % \vspace{0.20cm}
    \fig{width = 0.70\textwidth}{figs/checker-polysi}
    % \vspace{0.20cm}

    % \uncover<3->{
    % \polysi{} found SI violations in production database systems \\[2pt]
    % and provided understandable counterexamples.
    % }
  \end{center}
\end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Dependency Graph based Characterization of SI}
	\begin{center}
		\uncover<2->{
			\begin{tikzpicture}\node () {\includegraphics[scale = 0.10]{figs/1-logo}}; \end{tikzpicture}
			\teal{How to search for such a dependency graph efficiently?}
		}

		\vspace{1.00cm}
		\teal{\bf Enumerate} all possible \red{$\WW$} relations \\[3pt]
		and check whether any of them satisfies \blue{the ``cycle'' condition}.
		\vspace{1.00cm}

		\uncover<3->{
			\begin{tikzpicture}\node () {\includegraphics[scale = 0.10]{figs/2-logo}}; \end{tikzpicture}
			\blue{How to detect the ``cycle'' condition efficiently?}
		}
	\end{center}
\end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Dependency Graph based Characterization of SI}
	\begin{center}
		\begin{tikzpicture}\node () {\includegraphics[scale = 0.10]{figs/2-logo}}; \end{tikzpicture}
		\blue{How to detect the ``cycle'' condition efficiently?}
	\end{center}

  \begin{theorem}[Theorem 4.1 of~\ncite{AnalysingSI:JACM2018}]
		For a history \emph{$\H = (\T, \SO)$},
		\emph{\begin{align*}
			\H \models \si & \iff \H \models \intaxiom \;\land \\
				&\red{\exists}\; \WR, \WW, \RW.\; \G = (\H, \WR, \WW, \RW) \;\land \\
				&\quad (((\SO_{\G} \cup \WR_{\G} \cup \WW_{\G}) \comp \RW_{\G}?) \text{\it\; is acyclic}).
		\end{align*}}
  \end{theorem}

	\pause
	\begin{columns}
		\column{0.50\textwidth}
			{\fig{width = 1.00\textwidth}{figs/banking-lost-update-depgraph}}
		\column{0.50\textwidth}
			{\fig{width = 1.00\textwidth}{figs/banking-lost-update-depgraph}}
	\end{columns}
\end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Dependency Graph based Characterization of SI}
  \begin{center}
		\begin{tikzpicture}\node () {\includegraphics[scale = 0.10]{figs/1-logo}}; \end{tikzpicture}
		\teal{How to search for such a dependency graph efficiently?}

		\begin{columns}
			\column{0.50\textwidth}
				\pause
				{\fig{width = 1.00\textwidth}{figs/banking-lost-update-depgraph}}
			\column{0.50\textwidth}
				\pause
				\fig{width = 1.00\textwidth}{figs/sat-solver}
				\pause
				\red{encode the $\WW$ dependencies}
		\end{columns}
  \end{center}
\end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Polygraphs: A Family of Dependency Graphs}
	\fig{width = 0.30\textwidth}{figs/polygraph-history}
	\pause
	\begin{columns}
		\column{0.50\textwidth}
			\fig{width = 0.60\textwidth}{figs/polygraph-k-i}
		\column{0.50\textwidth}
		  \pause
			\fig{width = 0.60\textwidth}{figs/polygraph-i-k}
	\end{columns}
	\pause
	\fig{width = 0.30\textwidth}{figs/polygraph}
	\begin{center}
		polygraph: $\tuple{\eithervar \triangleq T_{k} \rel{\WW} T_{i},
		  \orvar \triangleq T_{j} \rel{\RW} T_{k}}$
	\end{center}
\end{frame}
%%%%%%%%%%%%%%%%%%%%

%%%%%%%%%%%%%%%%%%%%
\begin{frame}{Polygraphs: A Family of Dependency Graphs}
	\begin{columns}[c]
		\column{0.50\textwidth}
			\begin{center}
				\resizebox{0.60\textwidth}{!}{\input{tikz/g-polygraph-T-S-tikz}}
			\end{center}
		\column{0.50\textwidth}
			\begin{center}
				\resizebox{0.60\textwidth}{!}{\input{tikz/g-polygraph-S-T-tikz}}
			\end{center}
	\end{columns}
	\uncover<4->{
		\fig{width = 0.30\textwidth}{figs/g-polygraph}
		\begin{center}
			generalized polygraph: \\[5pt]
			$\tuple{\eithervar \triangleq \set{T \rel{\WW} S, T' \rel{\RW} S},
				    \orvar \triangleq \set{S \rel{\WW} T, S' \rel{\RW} T}}$
		\end{center}
	}
\end{frame}
%%%%%%%%%%%%%%%%%%%%