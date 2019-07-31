def read_pfam(file_handle,query_number):
    line = file_handle.readline()
    query_find=False #the indicate whether we have find the

    seqs = {}
    ids = OrderedDict()
    gs = {}
    gr = {}
    gf = {}
    gc = {}
    passed_end_alignment = False

    #assume the query only appears once in the document

    while line and not query_find: #find the line where the query number would be there
        if line[:7] == "#=GF AC":
            AC = line[7:].strip().split(".",1)[0]
            if AC == query_number:
                query_find = True
                break;
        line=file_handle.readline()

    while not passed_end_alignment and query_find and line: #when the chunk the alignment is found
        if line[:2] == "//": #the end of the alignment and leave the loop
            passed_end_alignment = True
            break;
        elif line == "": # blank line, ignore
            pass
        elif line[0] != "#":
            # Sequence
            # Format: "<seqname> <sequence>"
            assert not passed_end_alignment
            parts = [x.strip() for x in line.split(" ", 1)]
            if len(parts) != 2:
                # This might be someone attempting to store a zero length sequence?
                raise ValueError(
                    "Could not split line into identifier "
                    "and sequence:\n" + line)
            seq_id, seq = parts
            if seq_id not in ids:
                ids[seq_id] = True
                seqs.setdefault(seq_id, '')
                seqs[seq_id] += seq.replace(".", "-")
        elif len(line) >= 5:
            # Comment line or meta-data
            if line[:5] == '#=GC ':
            # Generic per-Column annotation, exactly 1 char per column
            # Format: "#=GC <feature> <exactly 1 char per column>"
                feature, text = line[5:].strip().split(None, 2)
                if feature not in gc:
                    gc[feature] = ""
                gc[feature] += text.strip()  # append to any previous entry
                    # Might be interleaved blocks, so can't check length yet
            elif line[:5] == '#=GS ':
            # Generic per-Sequence annotation, free text
            # Format: "#=GS <seqname> <feature> <free text>"
                seq_id, feature, text = line[5:].strip().split(None, 2)
                # if seq_id not in ids:
                #    ids.append(seq_id)
                if seq_id not in gs:
                    gs[seq_id] = {}
                if feature not in gs[seq_id]:
                    gs[seq_id][feature] = [text]
                else:
                    gs[seq_id][feature].append(text)
            elif line[:5] == "#=GR ":
            # Generic per-Sequence AND per-Column markup
            # Format: "#=GR <seqname> <feature> <exactly 1 char per column>"
                seq_id, feature, text = line[5:].strip().split(None, 2)
                # if seq_id not in ids:
                #    ids.append(seq_id)
                if seq_id not in gr:
                    gr[seq_id] = {}
                if feature not in gr[seq_id]:
                    gr[seq_id][feature] = ""
                gr[seq_id][feature] += text.strip()  # append to any previous entry
        line=file_handle.readline()

    assert len(seqs) <= len(ids)

    return ids,seqs

import gzip
import sys
from collections import OrderedDict

if __name__ == '__main__':
    pfam_file = sys.argv[1] #by default assume it's in gzip format
    query = sys.argv[2]
    pfam = gzip.open(pfam_file,"rt")

    ids, seqs = read_pfam(pfam, query)

    for seq_ids in ids:
        seq = seqs[seq_ids]
        print(seq_ids+"\t"+seq)
