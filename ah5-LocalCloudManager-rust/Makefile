# REFERENCE: the original core-systems package this module was relocated from, read only for source hashing.
# It is no longer in the workspace, so there is no default; docs/source-manifest.json is a historical record.
REFERENCE ?=
.PHONY: build test check docs
build:
	cargo build --locked --bin LocalCloudManager
test:
	cargo test --locked
check:
	cargo fmt -- --check
	cargo clippy --locked --all-targets -- -D warnings
docs:
	@test -n "$(REFERENCE)" && test -d "$(REFERENCE)" || { echo "make docs needs REFERENCE=<path to the original core-systems package>; it rewrites docs/source-manifest.json (see README.md)" >&2; exit 2; }
	cargo run --locked --bin localcloudmanager-documentation -- "$(REFERENCE)"
	mkdir -p ../ah5-support-runtime-rust/tmp/localcloudmanager-latex ../ah5-support-runtime-rust/output/pdf
	cd ../ah5-support-runtime-rust/documentation && pdflatex -interaction=nonstopmode -halt-on-error -output-directory=../tmp/localcloudmanager-latex LocalCloudManager/v5.2.2/LocalCloudManager-Rust-Catalogue.tex > ../tmp/localcloudmanager-latex/pass1.txt
	cd ../ah5-support-runtime-rust/documentation && pdflatex -interaction=nonstopmode -halt-on-error -output-directory=../tmp/localcloudmanager-latex LocalCloudManager/v5.2.2/LocalCloudManager-Rust-Catalogue.tex > ../tmp/localcloudmanager-latex/pass2.txt
	cd ../ah5-support-runtime-rust/documentation && pdflatex -interaction=nonstopmode -halt-on-error -output-directory=../tmp/localcloudmanager-latex LocalCloudManager/v5.2.2/LocalCloudManager-Rust-Catalogue.tex > ../tmp/localcloudmanager-latex/pass3.txt
	cp ../ah5-support-runtime-rust/tmp/localcloudmanager-latex/LocalCloudManager-Rust-Catalogue.pdf ../ah5-support-runtime-rust/output/pdf/
	cd ../ah5-support-runtime-rust/documentation && for source in LocalCloudManager/v5.2.2/*.tex; do pdflatex -interaction=nonstopmode -halt-on-error -output-directory=../tmp/localcloudmanager-latex "$$source" > ../tmp/localcloudmanager-latex/individual-last.txt || exit 1; done
