#include <iostream>
#include <set>
#include <algorithm>
#include <map>

using namespace std;

using Group = set<set<int>>::iterator;

class Fans {
public:
    explicit Fans(int n) {
        for(int i = 0; i < n; i++) {
            groups.insert({i});
        }
    }

    void MergeGroups(int fan1, int fan2) {
        auto group1 = GetGroup(fan1);
        auto group2 = GetGroup(fan2);
        if(group1 == group2) return;

        auto node1 = groups.extract(group1);
        auto node2 = groups.extract(group2);

        set<int>& temp1 = node1.value();
        set<int>& temp2 = node2.value();

        temp1.merge(temp2);
        groups.insert(move(temp1));
    }

    set<set<int>> GetCurrentGroups() const {
        return groups;
    }

    multiset<int> GetNumberFansInGroup() const {
        multiset<int> result;
        for(const auto& group : groups) {
            result.insert(group.size());
        }
        return result;
    }

private:
    Group GetGroup(int fan) {
        auto predicate = [=](const set<int>& s){return s.count(fan) > 0;};
        return find_if(groups.begin(), groups.end(),predicate);
    }

    set<set<int>> groups;
};

class Taxi {
public:
    void AddCar(pair<int,int> car) {
        if(car.second > 0 && car.first > 0) {
            cars[car.first] = car.second;
            cars_count += car.second;
        }
    }

    bool CombineCarsWithFans(multiset<int> fans) {
        if(cars.size() == 0 && fans.size() == 0) return true;
        if(cars.size() == 0) return false;
        if(static_cast<size_t>(cars_count) < fans.size()) return false;

        for(auto it = fans.rbegin(); it != fans.rend(); it++) {
            auto c_it = prev(cars.end());
            if(c_it->first < *it) return false;
            c_it->second--;
            if(c_it->second == 0) cars.erase(c_it);
        }
        return true;
    }

private:
    map<int,int> cars;
    int cars_count = 0;
};

int main()
{
    int n,m,k;

    cin >> n;
    Fans fans(n);

    cin >> m;
    for(int i = 0; i < m; i++) {
        int fan1,fan2;
        cin >> fan1 >> fan2;
        fans.MergeGroups(fan1,fan2);
    }

    cin >> k;
    Taxi tax;
    for(int i = 0; i < k; i++) {
        int cap,count;
        cin >> cap >> count;
        tax.AddCar({cap,count});
    }

    cout << tax.CombineCarsWithFans(fans.GetNumberFansInGroup());

    return 0;
}
